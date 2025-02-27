
  - [Spring Batch](#spring-batch)
  - [Job y Step](#job-y-step)
  - [Tasklet](#tasklet)
  - [Hello World](#hello-world)
  - [Scheduled](#scheduled)
  
## Spring Batch

Es un framework de spring enfocado específicamente en la creación de procesos batch. Los procesos batch (o procesos por lotes) acostumbran a ser aquellos programas que se lanzan bajo una determinada planificación y por lo tanto no requieren ningún tipo de intervención humana.


Para configurar Spring Data JPA en un proyecto, debemos agregar la siguiente dependencia en el archivo pom.xml:

```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-batch</artifactId>
</dependency>
```


Es necesario poner en dependencias de una base de datos hsqldb o h2, tambien se puede habilitar una especifica.

Una vez se arranca una aplicación con Spring Batch, se establece una conexión con la base de datos que contiene el esquema de tablas que utiliza el framework. Si no existe, se puede incluir por configuración que sea el propio framework el que cree el esquema de base de datos.

## Job y Step

Job -> El Job es la representación del proceso. Un proceso, a su vez, es un contenedor de pasos (steps).
Step ->  Un step (paso) es un elemento independiente dentro de un Job (un proceso) que representa una de las fases de las que está compuesto dicho proceso. Un proceso (Job) debe tener, al menos, un step.


Cada uno de estos steps suele constar de tres partes:

**ItemReader**: se encarga de la lectura del procesamiento por lotes. Esta lectura puede ser, por ejemplo, de una base datos; o también podría ser de un broker de mensajes o bien un fichero csv, xml, json, etc.
**ItemProccessor**: se encarga de transformar items previamente leídos. Esta transformación además de incluir cambios en el formato puede incluir filtrado de datos o lógica de negocio.
**ItemWriter**: este elemento es lo opuesto al itemReader. Se encarga de la escritura de los ítems. Esta puede ser inserciones en una base de datos, en un fichero csv, en un broker de mensajes, etc.

Está centrado a trabajar con los ítems de manera unitaria. Para el procesamiento por lotes podemos definir de qué tamaño será el número de ítems en el que se organizará el procesamiento por lotes. Si cogemos un tamaño 20, leerá, procesará y escribirá de 20 en 20. Este número de ítems que se procesarán en cada uno de los commits que realice el step se denomina chunk.

https://docs.spring.io/spring-batch/reference/step/chunk-oriented-processing.html


## Tasklet

Un step no tiene que estar compuesto por un reader, processor y writer. También puede tener únicamente una lógica de negocio. Es el caso del tasklet con el código que se desea ejecutar en el step.

En principio solo vamos a Job con tasklet.

https://docs.spring.io/spring-batch/reference/step/tasklet.html


## Hello World

Partiendo de un proyecto de spring boot con la dependencia de spring batch y h2 añadida en el pom(Ojo si usas otra base de datos, hay que cargar el schema.sql que está en la dependencia de spring batch core).

Definir un fichero de configuración, donde definirá los jobs y sus correspondientes steps.


```java
import org.springframework.batch.core.Job;
import org.springframework.batch.core.Step;
import org.springframework.batch.core.job.builder.JobBuilder;
import org.springframework.batch.core.repository.JobRepository;
import org.springframework.batch.core.step.builder.StepBuilder;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.transaction.PlatformTransactionManager;

@Configuration
@EnableBatchProcessing
@EnableScheduling
public class BatchConfig {

    private final JobRepository jobRepository;
    private final PlatformTransactionManager platformTransactionManager;
    private final HelloWorldTasklet helloWorldTasklet;

    public BatchConfig(JobRepository jobRepository, PlatformTransactionManager platformTransactionManager, HelloWorldTasklet helloWorldTasklet) {
        this.jobRepository = jobRepository;
        this.platformTransactionManager = platformTransactionManager;
        this.helloWorldTasklet = helloWorldTasklet;
    }

    @Bean
    public Job helloJob() {
        return new JobBuilder("helloJob", jobRepository)
                .start(step())
                .build();
    }

    @Bean
    protected Step step() {
        return new StepBuilder("step", jobRepository)
                .tasklet(helloWorldTasklet, platformTransactionManager)
                .build();
    }
}
```

```java

import org.springframework.batch.core.StepContribution;
import org.springframework.batch.core.scope.context.ChunkContext;
import org.springframework.batch.core.step.tasklet.Tasklet;
import org.springframework.batch.repeat.RepeatStatus;
import org.springframework.stereotype.Component;

@Component
public class HelloWorldTasklet implements Tasklet {

    @Override
    public RepeatStatus execute(StepContribution contribution, ChunkContext chunkContext) throws Exception{
        System.out.println("Hello world");
        return RepeatStatus.FINISHED;
    }
}
```

## Scheduled

En Spring, la anotación @Scheduled se utiliza para programar la ejecución de un método a intervalos regulares o en momentos específicos. Esta anotación es comúnmente utilizada en aplicaciones de Spring Framework para automatizar tareas programadas, como tareas de limpieza, generación de informes, actualizaciones de datos, entre otros.

_cron_

Permite especificar una expresión cron que define cuándo se debe ejecutar el método. Las expresiones cron son muy flexibles y pueden describir momentos precisos, intervalos regulares y patrones complejos de tiempo. Segundos, minutos, horas, días del mes, mes y días de la semana.

>    @Scheduled(cron = "0 0 12 * * ?") // Ejecuta el método todos los días a las 12:00 PM

_fixedDelay_

Indica un retraso fijo en milisegundos entre el final de la ejecución actual del método y el inicio de la próxima ejecución.
>     @Scheduled(fixedDelay = 5000) // Ejecuta el método cada 5 segundos después de que la ejecución anterior haya finalizado

Ejemplo de configuración para programar la ejecución de un JOB.
 
```java
import org.springframework.batch.core.Job;
import org.springframework.batch.core.JobParametersBuilder;
import org.springframework.batch.core.launch.JobLauncher;
import org.springframework.scheduling.annotation.Scheduled;
import org.springframework.stereotype.Component;

@Component
public class MySchedule {

    private final JobLauncher jobLauncher;
    private final Job helloJob;


    public MySchedule(JobLauncher jobLauncher, Job helloJob) {
        this.jobLauncher = jobLauncher;
        this.helloJob = helloJob;
    }

    @Scheduled(fixedDelayString = "${my.scheduled.task.fixed.delay}")
    public void performJob() throws Exception {
        jobLauncher.run(helloJob, new JobParametersBuilder()
                .addLong("time", System.currentTimeMillis()) // Parámetro único
                .toJobParameters());
    }

}
```

La configuración del cron o fixedDelay lo suyo es que ubicarla en el fichero en properties, para que se encuentre externalizada para agilizar los cambios.
CopyRight Alfonso

