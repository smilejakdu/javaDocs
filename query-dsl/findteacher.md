# FindTeacher

## FindTeacher

```java
    @Transactional(readOnly = true) // readOnly = true for read operations
    public FindTeacherResponseDto findAllTeacher(int page, int size) {
        List<TeacherDto> teacherDtos = queryFactory
                .select(Projections.constructor(TeacherDto.class,
                        qTeacher.id,
                        qTeacher.career,
                        qTeacher.user.email,
                        qTeacher.user.id,
                        qTeacher.skill,
                        qComments.likes.avg().coalesce(0.0),
                        qTeacher.createdAt
                ))
                .from(QTeacher.teacher)
                .leftJoin(QTeacher.teacher.comments, QComments.comments)
                .groupBy(QTeacher.teacher.id)
                .orderBy(QTeacher.teacher.user.email.asc())
                .offset((long) page * size)
                .limit(size)
                .fetch();

        long total = queryFactory
                .selectFrom(QTeacher.teacher)
                .fetchCount();

        int lastPage = (int) Math.ceil((double) total / size);

        return FindTeacherResponseDto.builder()
                .lastPage(lastPage)
                .teachers(teacherDtos)
                .build();
    }

```

위처럼 작성하게 되면 TeacherDto 랑 순서랑 값이 동일해야한다.

```java
@Data
@Builder
@AllArgsConstructor
@NoArgsConstructor
public class TeacherDto {
    private Long id;
    private String career;
    private String email;
    private Long userId;
    private String skill;
    private Double avgScore;
    private LocalDateTime createdAt;
}

```

작성했기 때문에 똑같이 select 값을 동일하게 해줘야한다. 그렇지 않다면 에러가 발생하게 됩니다.

