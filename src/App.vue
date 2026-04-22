<template>
  <div class="container">
    <h1>🚀 Vue3 + GitHub Actions CI/CD</h1>
    <p>这是一个用于学习 CI/CD 自动化部署的演示项目</p>
    
    <div class="status">
      ✨ 构建时间：{{ buildTime }}
    </div>

    <a 
      href="https://github.com/actions" 
      target="_blank" 
      class="github-link"
    >
      查看 GitHub Actions 文档
    </a>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      buildTime: new Date().toLocaleString('zh-CN')
    }
  }
}
</script>

<style scoped>
.container {
  max-width: 800px;
  margin: 0 auto;
}

h1 {
  font-size: 2.5rem;
  margin-bottom: 20px;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.2);
}

p {
  font-size: 1.2rem;
  margin-bottom: 30px;
  opacity: 0.9;
}

.status {
  background: rgba(255, 255, 255, 0.2);
  padding: 15px 30px;
  border-radius: 10px;
  display: inline-block;
  font-weight: bold;
  margin-bottom: 20px;
}

.github-link {
  margin-top: 20px;
  padding: 10px 25px;
  background: rgba(255, 255, 255, 0.25);
  color: white;
  text-decoration: none;
  border-radius: 5px;
  transition: background 0.3s;
}

.github-link:hover {
  background: rgba(255, 255, 255, 0.35);
}
</style>
