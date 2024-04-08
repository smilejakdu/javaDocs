# 초기셋팅



## Build gradle



```java
plugins {
    id 'java'
    id 'org.springframework.boot' version '3.0.1'
    id 'io.spring.dependency-management' version '1.1.0'
    // querydsl관련 명령어를 gradle탭에 생성해준다. (권장사항)
}


dependencies {
  ....
  // QueryDSL Implementation
  implementation 'com.querydsl:querydsl-jpa:5.0.0:jakarta'
  annotationProcessor "com.querydsl:querydsl-apt:${dependencyManagement.importedProperties['querydsl.version']}:jakarta"
  annotationProcessor "jakarta.annotation:jakarta.annotation-api"
  annotationProcessor "jakarta.persistence:jakarta.persistence-api"
}

/**
 * QueryDSL Build Options
 */
def querydslDir = "src/main/generated"

sourceSets {
    main.java.srcDirs += [ querydslDir ]
}


tasks.withType(JavaCompile) {
    options.getGeneratedSourceOutputDirectory().set(file(querydslDir))
}

clean.doLast {
    file(querydslDir).deleteDir()
}

```



## Service

```java
public class FindOneTeacherByIdApplication {
    private final JPAQueryFactory queryFactory;
    private final QTeacher qTeacher = QTeacher.teacher;
    private final QComments qComments = QComments.comments;
    
    public CoreResponse getTeacher() {
        Teacher teacher = queryFactory
            .selectFrom(qTeacher)
            .where(qTeacher.id.eq(teacherId))
            .leftJoin(qTeacher.comments, qComments)
            .fetchOne();
    }
```



이런식으로 사용할 수 있습니다.



