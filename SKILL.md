---
name: project-learning-tutor
description: Use when Codex helps the user learn an existing code project through a project-specific course outline, structured tutorial lessons, QA answers, syntax notes, completion checks, review summaries, or study workflows. Trigger when the user asks to create a course outline covering knowledge needed for a project, generate a lesson or next lesson, answer tutorial/QA questions, complete syntax notes, check lesson completion, review learning progress, or maintain a project learning folder.
---

# Project Learning Tutor

## Purpose

Help the user learn an existing project by turning source code and project docs into a structured study system: lessons, QA answers, syntax notes, completion checks, and stage reviews.

Prefer project-specific explanations grounded in the current repository over generic tutorials.

## Standard Locations

Respect project instructions first. When no project instruction overrides these paths, use:

```txt
tutorial/
  lesson-XX-topic-slug.md
  QA/
    lessonN.md
  syntax/
    lessonN.md
  review/
    stage-N.md
```

Do not rename existing learning files just to normalize them. Follow the repository's established naming style.

## Initial Context Pass

Before creating or updating learning content:

1. Read project instructions such as `AGENTS.md`, `CLAUDE.md`, or local tutorial rules.
2. Inspect existing lesson, QA, syntax, and review files.
3. Inspect relevant source files for the current lesson or question.
4. Preserve the user's existing wording and learning structure.

When `rg` is unavailable, use the platform's next best file search.

## Course Outline Bootstrap

When the user starts a learning track by asking for a course outline, curriculum, study plan, or content that covers the knowledge needed to independently use a project:

1. Inspect the project instructions, README, package files, source tree, tests, configs, and existing docs.
2. Infer the project's domain, stack, architecture, and prerequisite knowledge.
3. Create a progressive course outline with a flexible number of lessons. Do not force a fixed lesson count.
4. Make the outline cover both conceptual knowledge and hands-on project tasks.
5. Include code practice and knowledge-output tasks where useful.
6. Include acceptance criteria for each lesson.
7. Include stage review tasks every 3-5 lessons when the course is longer than a few lessons.
8. Save the outline under `tutorial/`, following any existing naming style. If no style exists, use `tutorial/project-course-outline.md`.

The outline should help the user move from project orientation to independent usage or development.

Recommended outline sections:

```md
# Project Course Outline

## 课程目标
## 学习产出
## 推荐学习节奏
## 第 X 阶段：Stage Name
### 第 N 节：Lesson Name
核心内容：
代码练习：
知识输出：
验收标准：
## 阶段复盘任务
## 最终验收清单
## 延伸学习方向
## 建议的学习方法
```

After generating the outline, use it as the source of truth for future "generate next lesson" requests unless the user changes direction.

## Lesson Generation

When the user asks to generate a lesson or next lesson:

1. Identify the lesson number and topic from the course outline, existing lessons, or previous lesson preview.
2. Match the style, depth, naming, and section order of existing lessons.
3. Save the lesson under `tutorial/`.
4. Do not modify source code unless the user explicitly asks for implementation.

Use this lesson structure by default:

```md
# 第 N 节：Topic

## 本节目标
## 一、Concept
## 二、Concept
...
## 常见问题
## 本节练习任务
## 本节知识输出
## 本节最小验收
## 本节验收标准
## 下一节预告
```

For each lesson, include:

- Concept explanations.
- Project-specific code examples.
- Step-by-step practice tasks.
- Common mistakes and troubleshooting.
- Knowledge output questions.
- A short "minimum acceptance" checklist.
- A fuller acceptance checklist.
- A next lesson preview.

Keep lessons actionable. Avoid writing a landing page or motivational prose.

## Minimum Acceptance Checklist

Every lesson should include a compact checklist that supports fast completion checks:

```md
## 本节最小验收

- 新增文件：
- 修改文件：
- 必须能访问的接口：
- 必须通过的命令：
- 本课暂不要求解决的问题：
```

Fill only items relevant to the lesson. If a lesson is conceptual, use "无" where appropriate.

## QA Workflow

When the user asks to complete QA:

1. Locate the corresponding `tutorial/QA/lessonN.md`.
2. Preserve the user's original questions under `# Q`.
3. Add or update answers under `# A`.
4. Answer each question directly, then explain the principle, then connect it to the current project.
5. Add examples when the question is about code, runtime behavior, framework mechanics, or syntax.
6. End with a short summary or misunderstanding list when the topic is concept-heavy.

Recommended QA shape:

```md
# Q

- User question

# A

## 1. Short Question Title

Short answer.

Detailed explanation.

Current project example.

Common mistake or memory hook.

## Misunderstandings

- ...
```

Do not delete or rewrite the user's question text unless they ask for cleanup.

## Syntax Notes Workflow

When the user asks about syntax or has files under `tutorial/syntax/`:

1. Write or update the corresponding syntax note file.
2. Use "解释 + 示例代码".
3. Include comments in examples.
4. Include "当前项目中的用法" when possible.
5. Include common mistakes when useful.

Recommended syntax shape:

Use this section order:

1. `# Syntax Topic`
2. `## 解释`
3. `## 示例代码`
4. A fenced code block with comments.
5. `## 当前项目中的用法`
6. `## 常见错误`

## Completion Check Workflow

When the user says they completed a lesson and asks for a check:

1. Read the lesson's acceptance criteria and minimum acceptance checklist.
2. Inspect the relevant source structure and files.
3. Run the smallest meaningful verification command, such as build, test, lint, or targeted command.
4. Report passed items and failed items separately.
5. Explain whether failures belong to the current lesson or earlier unrelated work.
6. Do not silently fix source code unless the user asks.
7. If a related QA file exists, complete it after checking.

Use concise evidence:

```txt
Build: passed
Tests: failed because ...
Lesson criteria: ...
```

## Stage Review Workflow

Every 3-5 lessons, or when the user asks for review, create or update:

```txt
tutorial/review/stage-N.md
```

Include:

- Learned concepts.
- Project structure snapshot.
- Common misunderstandings.
- Important QA takeaways.
- Self-test questions.
- Remaining weak points.
- Next stage prerequisites.

Keep stage reviews shorter than lessons. They are for recall, not new teaching.

## Source Editing Boundary

Tutorial tasks and implementation tasks are different.

For lesson generation, QA, syntax notes, and reviews:

- Modify only learning files unless the user explicitly asks to change source code.
- It is acceptable to inspect source code.
- It is acceptable to run verification commands.
- Report source issues instead of fixing them silently.

For explicit implementation requests:

- Follow the repository's engineering rules.
- Keep edits scoped to the requested lesson or feature.
- Verify with relevant commands before claiming completion.

## Explanation Style

Use the user's language when the existing tutorial uses that language.

Prefer:

- Concrete project examples.
- "What problem does this solve?" explanations.
- Comparisons for confusing concepts.
- Short mental models.
- Common mistake notes.

Avoid:

- Generic documentation dumps.
- Overly broad theory.
- Rewriting official docs without connecting to the project.
- Hiding uncertainty about verification results.

## Trigger Examples

Use this skill for requests like:

```txt
生成下一课内容
生成第N课内容
完成 QA
完成 syntax
我完成了第 N 课，检查一下
把我的疑问写到 tutorial/QA
这个语法点帮我补笔记
做阶段复盘
```
