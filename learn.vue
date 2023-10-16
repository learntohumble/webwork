<template>
  <div class="container">
    <div>
      <div v-for="post in posts" :key="post.id" class="post">
        <div class="post-header">
          <span class="post-author">{{ post.author }}</span>
          <span class="post-time">{{ post.time }}</span>
        </div>
        <div class="post-content">{{ post.content }}</div>
        <div class="post-action">
          <el-button @click="toggleLike(post.id)" size="small" :type="isLiked(post.id) ? 'danger' : 'primary'">
            {{ isLiked(post.id) ? '取消点赞' : '点赞' }}
          </el-button>
          <el-button @click="toggleComment(post.id)" size="small">评论</el-button>
          <div class="post-actions">
            <el-button class="delete-button" @click="deletePost(post.id)" size="small">删除</el-button>
          </div>
        </div>
        <div class="post-comments" v-show="showComments[post.id]">
          <div v-for="comment in post.comments" :key="comment.id" class="comment">
            <span class="comment-author">{{ comment.author }}</span>
            <span class="comment-content">{{ comment.content }}</span>
          </div>
          <div class="comment-form">
            <el-input v-model="newComment[post.id]" placeholder="添加评论"></el-input>
            <el-button @click="addComment(post.id)" size="small">发布</el-button>
          </div>
        </div>
      </div>
      <div class="post-form">
        <el-input type="textarea" v-model="newPostContent" placeholder="发表动态"></el-input>
        <el-button @click="addPost" type="primary" size="small">发布</el-button>
      </div>
    </div>
    
  </div>
</template>
<script>
import { useUserStore } from '../stores/user';
var name=useUserStore().user.name;

export default {
  data() {
    return {
      posts: [
       
      ],
      showComments: {},
      newComment: {},
      newPostContent: ''
    };
  },    
  methods: {
    toggleLike(postId) {
      const post = this.posts.find(post => post.id === postId);
      const index = post.likes.indexOf(name);
      if (index === -1) {
        post.likes.push(name);
      } else {
        post.likes.splice(index, 1);
      }
    },
    toggleComment(postId) {
        console.log(name);
      this.showComments[postId] = !this.showComments[postId];
    },
    addComment(postId) {
      const post = this.posts.find(post => post.id === postId);
      const content = this.newComment[postId];
      if (content) {
        post.comments.push({
          id: Date.now(),
          author: name,
          content: content
        });
        this.newComment[postId] = '';
      }
    },
    addPost() {
      const content = this.newPostContent;
      if (content) {
        this.posts.push({
          id: Date.now(),
          author: name,
          time: new Date().toLocaleString(),
          content: content,
          likes: [],
          comments: []
        });
        this.newPostContent = '';
      }
    },
    deletePost(postId) {
      // 根据postId执行删除操作
      // 在这里假设使用数组的splice方法来删除动态
      const index = this.posts.findIndex(post => post.id === postId);
      if (index !== -1) {
        this.posts.splice(index, 1);
      }
    },
    isLiked(postId) {
      const post = this.posts.find(post => post.id === postId);
      return post.likes.includes(name);
    }
  }
};
</script>
<style>
.container {
  width: 80%;
  margin: 0 auto;
}

.post {
  margin-bottom: 20px;
  padding: 10px;
  border: 1px solid #ccc;
}

.post-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.post-author {
  font-weight: bold;
}

.post-time {
  color: #888;
}

.post-content {
  margin-top: 10px;
}

.post-action {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-top: 10px;
}

.post-actions {
  margin-top: 10px;
}

.post-comments {
  margin-top: 10px;
}

.comment {
  margin-top: 5px;
}

.comment-author {
  font-weight: bold;
}

.comment-content {
  margin-left: 5px;
}

.comment-form {
  margin-top: 10px;
  display: flex;
  align-items: center;
}

.post-form el-input[type="textarea"] {
  width: 100%;
  height: 80px;
  margin-bottom: 10px;
}

.post-form el-button {
  display: block;
  margin-left: auto;
}
</style>