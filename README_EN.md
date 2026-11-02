# CS Paper Reader

[한국어](README.md)

A Codex skill for reading computer science papers and writing reviews using your project's template. Reviews default to Korean, with English also supported. The skill analyzes the problem, system design, implementation, evaluation, and limitations of local PDFs, then saves Markdown reviews in `reviews/`.

Key claims and numerical results include references to sections, pages, tables, or figures. Reviews distinguish the paper's statements from the reviewer's interpretation. The skill supports individual reviews, batch reviews of papers without existing reviews, and updates to existing reviews.

## Package Contents

```text
cs-paper-reader/
├── SKILL.md                 Skill instructions and triggers
├── agents/openai.yaml       Codex display metadata
├── assets/template_KO.md    Default Korean review template
├── assets/template_EN.md    English review template
├── README.md                Korean installation and usage guide
├── README_EN.md             English installation and usage guide
└── .gitignore
```

## Installation

Download the repository and run the following commands **from the directory containing this README**. They copy only the files needed to run the skill into your user skills directory and stop if a skill with the same name already exists.

### Windows PowerShell

```powershell
$skillTarget = Join-Path $HOME '.agents\skills\cs-paper-reader'
if (Test-Path -LiteralPath $skillTarget) {
    throw "The installation path already exists: $skillTarget"
}
New-Item -ItemType Directory -Path $skillTarget -Force | Out-Null
Copy-Item -LiteralPath @('SKILL.md', 'agents', 'assets') -Destination $skillTarget -Recurse
```

### macOS / Linux

```bash
skill_target="${HOME}/.agents/skills/cs-paper-reader"
if [ -e "$skill_target" ]; then
    printf '%s\n' "The installation path already exists: $skill_target" >&2
    exit 1
fi
mkdir -p "$skill_target"
cp -R SKILL.md agents assets "$skill_target/"
```

For a project-only installation, copy the same three items into that project's `.agents/skills/cs-paper-reader/` directory. If the skill does not appear, restart Codex. See the [official OpenAI skills documentation](https://learn.chatgpt.com/docs/build-skills) for installation locations and skill discovery.

## Prepare a Paper Workspace

Keep your paper workspace separate from the skill source.

```text
paper-workspace/
├── papers/
│   └── example.pdf
├── templates/
│   └── template_EN.md
└── reviews/
```

Place papers in `papers/`. For English reviews, copy the [English template](assets/template_EN.md) to `templates/template_EN.md` and customize it as needed. You can also copy the [Korean template](assets/template_KO.md) to `templates/template_KO.md`. Existing project templates take priority over bundled templates. If your project has no templates, the skill uses a bundled template for the review language. It creates `reviews/` when needed.

English reviews can be requested explicitly or selected by specifying the English template. Without a language request or an English template selection, reviews default to Korean.

## Usage Examples

From your paper workspace, request a review in English:

```text
$cs-paper-reader Review papers/example.pdf in English using templates/template_EN.md.
```

If your project has no templates, request English to use the bundled English template:

```text
$cs-paper-reader Review papers/example.pdf in English.
```

```text
$cs-paper-reader Review all papers that do not have a review yet. Write the reviews in English.
```

```text
$cs-paper-reader Update the existing review of example.pdf in English. Expand the analysis of experimental conditions and limitations.
```

When working from another directory, include the paper workspace's path. You can also specify a custom template, review language, output filename, and reading objectives.

The default output for `papers/example.pdf` is `reviews/example.md`. Subdirectory structure is preserved. New reviews do not overwrite existing reviews without authorization. Batch requests for papers without reviews skip papers that already have a review.

## Requirements

Codex needs access to local files, PDF text extraction tools, and page rendering tools. The skill uses available PDF skills and tools; scanned documents may require OCR. This package does not require a separate Python script or an API key.

Personal absolute paths, paper PDFs, and generated reviews are not included in the package.
