# Spring Redis

```java
package com.example.showmeyourability.comments.application;

import com.example.showmeyourability.comments.domain.Comments;
import com.example.showmeyourability.comments.infrastructure.dto.FindCommentDto.CommentDto;
import com.example.showmeyourability.comments.infrastructure.repository.CommentRepository;
import com.example.showmeyourability.shared.CoreSuccessResponse;
import lombok.RequiredArgsConstructor;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

@Service
@RequiredArgsConstructor
public class FindCommentByTeacherIdApplication {
    private final CommentRepository commentRepository;


    @Transactional
    public CoreSuccessResponse execute(
            Long teacherId
    ) {
        List<Comments> comments = commentRepository.findByTeacherId(teacherId);
        List<CommentDto> commentDtoList = getCommentDtoList(comments);
        return CoreSuccessResponse
                .builder()
                .ok(true)
                .message("댓글 조회 성공")
                .data(commentDtoList)
                .build();
    }

    private List<CommentDto> getCommentDtoList(List<Comments> comments) {
        ArrayList<CommentDto> commentDtoList = new ArrayList<>(Collections.emptyList());
        for (Comments comment : comments) {
            CommentDto commentDto = CommentDto.builder()
                    .userId(comment.getUser().getId())
                    .teacherId(comment.getTeacher().getId())
                    .likes(comment.getLikes())
                    .content(comment.getContent())
                    .build();
            commentDtoList.add(commentDto);
        }
        return commentDtoList;
    }
}

```

위의 코드는 comments 를 불러오는 코드이다 .

선생님에 따른 comments 들이 불러온다.

