根据提供的Git diff记录，以下是对代码变更的评审：

### `.github/workflows/main-maven-jar.yml`

**变更点：**
- 在GitHub Actions的工作流程文件中添加了一个新的环境变量 `PromptMessage`。

**评审：**
- **优点：** 添加 `PromptMessage` 环境变量提供了额外的灵活性，允许用户定义一个特定的提示信息，这可能会在OpenAI的代码审查过程中被使用。
- **缺点：** 没有在文档中说明 `PromptMessage` 的用途和预期格式，这可能导致后续维护人员或使用者困惑。

### `OpenAiCodeReview.java`

**变更点：**
- `OpenAiCodeReview` 类的构造函数现在接受一个额外的 `PromptMessage` 参数。

**评审：**
- **优点：** 这个变更允许在初始化 `OpenAiCodeReview` 实例时传递一个提示信息，这有助于在代码审查过程中提供上下文。
- **缺点：** 如果 `PromptMessage` 的内容或格式不正确，可能会导致代码审查失败或产生错误的结果。应该确保这个参数被正确验证和校验。

### `OpenAiCodeReviewService.java`

**变更点：**
- 在 `codeReview` 方法中添加了对 `PromptMessage` 的使用。

**评审：**
- **优点：** 这个变更利用了 `PromptMessage`，使得代码审查的上下文更加丰富。
- **缺点：** 在构造 `ChatCompletionRequestDTO` 时没有直接使用 `PromptMessage`，而是硬编码了提示信息。这可能会限制灵活性，并且如果需要修改提示信息，可能需要在多个地方进行更改。

### `ChatGLM.java`

**变更点：**
- `ChatGLM` 类的构造函数现在接受一个额外的 `promptMessage` 参数，并添加了一个 `getPromptMessage` 方法。

**评审：**
- **优点：** 这个变更保持了 `ChatGLM` 类的清晰性和可扩展性，允许在初始化时设置提示信息，并在需要时检索它。
- **缺点：** 如果 `promptMessage` 的内容不正确，可能会导致服务调用失败。确保 `promptMessage` 在使用前经过适当的验证。

### 总结

总体而言，这些变更提供了额外的灵活性和上下文，但同时也引入了一些潜在的风险。建议：
- 在文档中详细说明所有新环境变量和参数的用途和预期格式。
- 在代码中添加适当的错误处理和验证，以确保参数的正确性和服务的稳定性。