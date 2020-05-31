---
layout: post
title: Testcontainers
---

A menudo acudimos a una base de datos en memoria, como H2, para realizar pruebas de integración. Pero siempre es aconsejable usar el mismo motor de base de datos que utilizaremos en producción. Para ello nos puede ayudar Testcontainers que genera contenedores con la base de datos que necesitemos.

Podemos incluir Testcontainers en una aplicación de Spring Boot para realizar los test de integración. Para esta demostración utilizaremos Spring Boot 2.3, Junit 5, MySql y Testcontainer.

Agregamos las dependencias en el archivo pom.xml

```xml
<dependency>
    <groupId>org.testcontainers</groupId>
    <artifactId>testcontainers</artifactId>
    <version>1.14.2</version>
    <scope>test</scope>
</dependency>
<dependency>
    <groupId>org.testcontainers</groupId>
    <artifactId>junit-jupiter</artifactId>
    <version>1.14.2</version>
    <scope>test</scope>
</dependency>
<dependency>
    <groupId>org.testcontainers</groupId>
    <artifactId>mysql</artifactId>
    <version>1.14.2</version>
    <scope>test</scope>
</dependency>
```

Iniciamos el proyecto creando una entidad Contacto con algunos atributos como id, nombre y email.

```java
@Entity
public class Contacto {

    @Id
    private String id;
    private String nombre;
    private String email;

```

Escribimos el repositorio utilizando la librería Spring Data JPA que ya nos provee los métodos CRUD  básicos.

```java
public interface ContactoRepository extends JpaRepository<Contacto, String> {}
```

Creamos un archivo contactos.sql con datos de prueba.

```sql
insert into contacto (id, email, nombre)
values
('979177d9-1ff5-4c02-8dc6-0d2c60df8ffd', 
  'homer@email.com', 'Cosme Fulanito');
```

Antes de iniciar las pruebas, escribimos una clase abstracta que nos sirve de base a todas las pruebas de integración con MySQL.

Utilizamos un objeto MySQLContainer que nos permite iniciar un contenedor de MySQL al arrancar las pruebas. 

Además registramos los datos del contenedor en Spring utilizando un método anotado con DynamicPropertySource. Realizamos esta acción ya que no tenemos los atributos de la conexión hasta que no arrancamos el contenedor. 

```java
abstract class AbstractContainerBaseTest {

    static final MySQLContainer MY_SQL_CONTAINER;

    static {
        MY_SQL_CONTAINER = new MySQLContainer();
        MY_SQL_CONTAINER.start();
    }

    @DynamicPropertySource
    static void mysqlProperties(DynamicPropertyRegistry registry) {
        registry.add("spring.datasource.url",
                      MY_SQL_CONTAINER::getJdbcUrl);
        registry.add("spring.datasource.username",
                      MY_SQL_CONTAINER::getUsername);
        registry.add("spring.datasource.password",
                      MY_SQL_CONTAINER::getPassword);
    }

}
```

Ahora podemos escribir nuestro test de integración contra una base de datos MySQL real.

```java
@RunWith(SpringRunner.class)
@SpringBootTest
class ContactoRepositoryTest extends AbstractContainerBaseTest {

    @Autowired
    private ContactoRepository contactoRepository;

    @Test
    @Sql("/sql/contactos.sql")
    public void deberiaConsultarContacto() {
        String id = "979177d9-1ff5-4c02-8dc6-0d2c60df8ffd";
        Contacto contacto = contactoRepository.findById(id).get();
        assertThat(contacto.getEmail()).isEqualTo("homer@email.com");
        assertThat(contacto.getNombre()).isEqualTo("Cosme Fulanito");
    }

}
```

Para ejecutar los test simplemente escribimos

```bash
mvn test
```

## Referencias

- [Testcontainer](https://www.testcontainers.org/)
- [DynamicPropertySource](https://spring.io/blog/2020/03/27/dynamicpropertysource-in-spring-framework-5-2-5-and-spring-boot-2-2-6)