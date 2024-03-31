# 데이터 변경시 캐시 무효화



데이터가 변경이 되면 ,&#x20;

redis. 의 데이터와

데이터베이스에 들어있는 데이터가 상이할 수 있습니다.

이럴경우 redis 에 들어있는 데이터를 초기화 시켜줍니다.

```java
package com.example.showmeyourability.comments.application;

import com.example.showmeyourability.comments.domain.Comments;
import com.example.showmeyourability.comments.domain.ContentVo;
import com.example.showmeyourability.comments.infrastructure.dto.UpdateCommentDto.UpdateCommentReqeustDto;
import com.example.showmeyourability.comments.infrastructure.dto.UpdateCommentDto.UpdateCommentResponseDto;
import com.example.showmeyourability.comments.infrastructure.repository.CommentRepository;
import com.example.showmeyourability.shared.Exception.HttpExceptionCustom;
import lombok.RequiredArgsConstructor;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.http.HttpStatus;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

import java.time.LocalDateTime;

@Service
@RequiredArgsConstructor
public class UpdateCommentApplication {
    private static final String ERROR_COMMENT_NOT_FOUND = "요청한 commentId에 해당하는 댓글이 존재하지 않습니다.";
    private static final String CACHE_KEY_PATTERN = "comments:teacher:%d";

    private final CommentRepository commentRepository;
    private final RedisTemplate<String, Object> redisTemplate;

    @Transactional
    public UpdateCommentResponseDto execute(
            Long commentId,
            UpdateCommentReqeustDto request
    ) {
        Comments comment = commentRepository.findById(commentId)
                .orElseThrow(() -> new HttpExceptionCustom(false, ERROR_COMMENT_NOT_FOUND, HttpStatus.BAD_REQUEST));

        comment.setContent(request.getContent());
        comment.setUpdatedAt(LocalDateTime.now());
        invalidateRelatedCache(comment.getTeacher().getId());

        return toResponseDto(comment);
    }

    private void invalidateRelatedCache(Long teacherId) {
        String cacheKey = generateCacheKey(teacherId);
        redisTemplate.delete(cacheKey);
    }

    private String generateCacheKey(Long teacherId) {
        return String.format(CACHE_KEY_PATTERN, teacherId);
    }

    private UpdateCommentResponseDto toResponseDto(Comments comment) {
        return UpdateCommentResponseDto.builder()
                .userId(comment.getUser().getId())
                .teacherId(comment.getTeacher().getId())
                .likes(comment.getLikes()) // 가정: Likes는 VO 객체
                .content(comment.getContent()) // 가정: Content는 VO 객체
                .build();
    }
}

```

