name: Update Wiki Page

on:
  workflow_dispatch:
    inputs:
      branch:
        description: 'Wiki repository branch to push to'
        required: false
        default: 'main'
      filename:
        description: 'Wiki page filename'
        required: false
        default: 'Awesome+线上教育.md'

permissions:
  contents: write

jobs:
  update-wiki:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout wiki repository
        uses: actions/checkout@v4
        with:
          repository: zhangmengru752-pixel/AA.wiki
          token: ${{ secrets.WIKI_PAT }}
          path: wiki

      - name: Create or update wiki page
        env:
          WIKI_BRANCH: ${{ github.event.inputs.branch || 'main' }}
          FILE_NAME: ${{ github.event.inputs.filename || 'Awesome+线上教育.md' }}
        run: |
          cat > wiki/"$FILE_NAME" <<'EOF'
# Awesome+线上教育

这是一个收集线上教育（在线学习、慕课、教学平台、课程资源）优质资源的索引页面。目标是汇聚高质量课程、平台、教学工具与参考文章，方便学习者和教育工作者快速找到合适的资源。

## 目录
- 课程平台
- 免费课程
- 专业路线与学习路径
- 教学工具与平台
- 研究与报告
- 贡献者

## 课程平台
- Coursera — 大量大学课程与专业证书
- edX — 大学与科研机构课程
- Udacity — 强调项目与实践的职业课程
- 国内平台：慕课网、网易云课堂、极客时间、腾讯课堂

## 免费课程
- CS50（哈佛） — 计算机科学入门
- Machine Learning（Andrew Ng） — 机器学习入门
- Stanford CS 系列 — 高质量大学课程

## 专业路线与学习路径
- 数据科学路线：数学（线代/概率/统计） -> 编程（Python/R） -> 数据分析 -> 机器学习 -> 项目实践
- 前端开发：HTML/CSS -> JavaScript -> 框架（React/Vue） -> 构建工具 -> 项目实战

## 教学工具与平台
- LMS：Moodle、Canvas
- 直播与互动：Zoom、腾讯会议、钉钉
- 作业与评估：Gradescope、自动化测试（CI）工具

## 研究与报告
- 学习分析（Learning Analytics）相关论文与报告链接（可补充具体论文/报告链接）

## 贡献者
欢迎提交 PR 或在本页下方提出建议与资源。请在贡献时附上资源描述与来源链接。

---
如需我���页面改为英文、拆分为多个主题页面（例如“免费课程集合”、“职业路线”各自独立页面），或把每一项扩展为带链接和说明的条目，我可以帮您把内容扩充成更详尽的版本。EOF

          cd wiki
          git config user.name "github-actions[bot]"
          git config user.email "github-actions[bot]@users.noreply.github.com"
          git add "$FILE_NAME"
          git commit -m "Add/Update $FILE_NAME" || echo "No changes to commit"
          git push origin HEAD:"$WIKI_BRANCH"
