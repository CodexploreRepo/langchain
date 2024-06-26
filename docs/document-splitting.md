# Document Splitting

- Split documents into smaller chunks
  - `chunk_size` the length of the chunk
  - `chunk_overlap` overlap between 2 chunks, or sliding windows
- Retaining meaningful relationships
  - Example:
    - Original Sentence: ".. on this model. The Toyota Camry has a head-snapping 80 HP and an eight-speed automatic transmission that will .."
    - Chunk 1: "on this model. The Toyota Camry has a head-snapping"
    - Chunk 2: "80 HP and an eight-speed automatic transmission that will"

## Types of Text Splitters

- LangChain offers many different types of `text splitters`.
- Table columns:
  - **Name**: Name of the text splitter
  - **Classes**: Classes that implement this text splitter
  - **Splits On**: How this text splitter splits text
  - **Adds Metadata**: Whether or not this text splitter adds metadata about where each chunk came from.
  - **Description**: Description of the splitter, including recommendation on when to use it.

| Name      | Classes                                                          | Splits On                             | Adds Metadata | Description                                                                                                                                               |
| --------- | ---------------------------------------------------------------- | ------------------------------------- | ------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Recursive | `RecursiveCharacterTextSplitter` :star:, `RecursiveJsonSplitter` | A list of user defined characters     |               | Recursively splits text. This splitting is trying to keep related pieces of text next to each other. This is the recommended way to start splitting text. |
| HTML      | `HTMLHeaderTextSplitter`, `HTMLSectionSplitter`                  | HTML specific characters              | ✅            | Splits text based on HTML-specific characters. Notably, this adds in relevant information about where that chunk came from (based on the HTML)            |
| Markdown  | `MarkdownHeaderTextSplitter`                                     | Markdown specific characters          | ✅            | Splits text based on Markdown-specific characters. Notably, this adds in relevant information about where that chunk came from (based on the Markdown)    |
| Code      | many languages                                                   | Code (Python, JS) specific characters |               | Splits text based on characters specific to coding languages. 15 different languages are available to choose from.                                        |
