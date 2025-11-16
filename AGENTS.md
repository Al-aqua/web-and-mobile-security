# Agent Guidelines for Web and Mobile Security Course Materials

## Build/Preview Commands
- **Build lab manual**: `mdbook build` (outputs to `book/`)
- **Preview lab manual**: `mdbook serve` (live at http://localhost:3000)
- **Watch mode**: `mdbook watch` (auto-rebuild on changes)
- **Build slides**: `marp powerpoints/<file>.md -o powerpoints/<file>.pdf` or `marp powerpoints/<file>.md -o powerpoints/<file>.html`
- **Watch slides**: `marp -w powerpoints/<file>.md`

## Project Structure
- `outline.md` - **Course outline defining all chapters, theory lectures, and practical labs**
- `src/` - mdBook lab manuals (Markdown)
- `powerpoints/` - Marp lecture slides (Markdown with Marp directives)
- `theme/` - mdBook custom theme (CSS, fonts, JS)

## Content Guidelines
- **Course outline**: All content MUST be based on `outline.md` which defines 10 chapters with theory lectures and practical labs
- **Research first**: ALWAYS research topics thoroughly before writing any content using web searches or documentation
- **Lab manuals**: Keep each lab 2-5 pages when printed; focus on practical, hands-on exercises
- **Theory lectures**: Create PowerPoint slides in `powerpoints/` following the topics in `outline.md`
- **Printable format**: Ensure labs are print-friendly (clear structure, minimal colors, proper page breaks)
- **Slides**: Use Marp syntax (`---` for slides, `<!-- -->` for speaker notes)
- **Security focus**: Reference OWASP standards, use real-world scenarios, include security best practices

## Writing Style
- Clear, instructional tone for labs with step-by-step commands
- Academic but accessible for lectures
- Include code blocks with proper syntax highlighting
- Provide context for security concepts and attack vectors
