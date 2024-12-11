<!-- CommentSection.vue -->
<template>
  <div class="comment-section">
    <!-- 댓글 입력 폼 -->
    <div class="comment-form">
      <h3>댓글 작성</h3>
      <textarea
        v-model="newComment"
        placeholder="댓글을 입력하세요..."
        class="comment-input"
        rows="3"
      ></textarea>
      <button @click="addComment" class="submit-btn">댓글 작성</button>
    </div>

    <!-- 댓글 목록 -->
    <div class="comments-">
      <h3>댓글 목록 ({{ comments.length }})</h3>
      <div v-if="comments.length === 0" class="no-comments">
        아직 작성된 댓글이 없습니다.
      </div>
      <div v-else>
        <div v-for="comment in comments" :key="comment.id" class="comment-item">
          <div class="comment-header">
            <span class="author">{{ comment.author }}</span>
            <span class="date">{{ formatDate(comment.date) }}</span>
          </div>
          <div class="comment-content">
            <p>{{ comment.content }}</p>
          </div>
          <div class="comment-actions">
            <button
              v-if="isAuthor(comment)"
              @click="deleteComment(comment.id)"
              class="delete-btn"
            >
              삭제
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';

// 댓글 데이터 상태 관리
const comments = ref([
  {
    id: 1,
    author: '홍길동',
    content: '첫 번째 댓글입니다!',
    date: new Date('2024-12-11'),
  },
  {
    id: 2,
    author: '김철수',
    content: '좋은 글이네요!',
    date: new Date('2024-12-12'),
  },
]);

// 새 댓글 입력값 관리
const newComment = ref('');

// 현재 로그인한 사용자 (실제로는 인증 시스템과 연동 필요)
const currentUser = {
  name: '홍길동',
};

// 날짜 포맷팅 함수
const formatDate = (date) => {
  return new Date(date).toLocaleDateString('ko-KR', {
    year: 'numeric',
    month: 'long',
    day: 'numeric',
  });
};

// 댓글 작성자 확인
const isAuthor = (comment) => {
  return comment.author === currentUser.name;
};

// 댓글 추가
const addComment = () => {
  if (!newComment.value.trim()) return;

  const comment = {
    id: comments.value.length + 1,
    author: currentUser.name,
    content: newComment.value,
    date: new Date(),
  };

  comments.value.push(comment);
  newComment.value = '';
};

// 댓글 삭제
const deleteComment = (commentId) => {
  comments.value = comments.value.filter((comment) => comment.id !== commentId);
};
</script>

<style scoped>
.comment-section {
  max-width: 800px;
  padding: 20px 0;
}

.comment-form {
  margin-bottom: 30px;
}

.comment-input {
  width: 100%;
  padding: 12px;
  margin: 10px 0;
  border: 1px solid #ddd;
  border-radius: 4px;
  resize: vertical;
}

.submit-btn {
  padding: 8px 16px;
  background-color: #4caf50;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.submit-btn:hover {
  background-color: #45a049;
}

.comments- {
  margin-top: 20px;
}

.comment-item {
  border-bottom: 1px solid #eee;
  margin: 15px 0;
}

.comment-header {
  display: flex;
  justify-content: space-between;
  margin-bottom: 8px;
}

.author {
  font-weight: bold;
}

.date {
  color: #666;
}

.comment-content {
  margin: 10px 0;
}

.comment-actions {
  display: flex;
  justify-content: flex-end;
}

.delete-btn {
  padding: 4px 8px;
  background-color: #ff4444;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.delete-btn:hover {
  background-color: #cc0000;
}

.no-comments {
  text-align: center;
  color: #666;
  padding: 20px;
}
</style>
