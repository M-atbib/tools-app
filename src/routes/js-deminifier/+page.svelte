<script lang="ts">
  import { onMount } from "svelte";

  let inputCode: string = "";
  let outputCode: string = "";

  // Function to beautify JavaScript code
  function beautifyCode() {
    if (!inputCode.trim()) return;

    try {
      // Basic beautification rules
      outputCode = inputCode
        // Add space after commas
        .replace(/,/g, ", ")
        // Add space around operators
        .replace(/([+\-*/%=<>!&|])/g, " $1 ")
        // Add newline and indent after opening braces
        .replace(/{/g, "{\n    ")
        // Add newline before closing braces
        .replace(/}/g, "\n}")
        // Add newline and indent after semicolons
        .replace(/;/g, ";\n    ")
        // Add space after keywords
        .replace(/(if|for|while|function|return|var|let|const)\(/g, "$1 (")
        // Add newlines between statements
        .replace(/}\s*else/g, "}\nelse")
        .replace(/}\s*catch/g, "}\ncatch")
        // Clean up extra whitespace
        .replace(/\s+\n/g, "\n")
        .replace(/\n\s+}/g, "\n}")
        .replace(/\n{3,}/g, "\n\n")
        .trim();
    } catch (error: unknown) {
      if (error instanceof Error) {
        alert("Error beautifying code: " + error.message);
      } else {
        alert("Error beautifying code");
      }
    }
  }

  // Function to clear input
  function clearInput() {
    inputCode = "";
    outputCode = "";
  }

  // Function to copy to clipboard
  async function copyToClipboard() {
    if (!outputCode) return;

    try {
      await navigator.clipboard.writeText(outputCode);
      alert("Code copied to clipboard!");
    } catch (error: unknown) {
      if (error instanceof Error) {
        alert("Failed to copy code: " + error.message);
      } else {
        alert("Failed to copy code");
      }
    }
  }

  // Function to download beautified code
  function downloadCode() {
    if (!outputCode) return;

    const blob = new Blob([outputCode], { type: "text/javascript" });
    const url = URL.createObjectURL(blob);
    const a = document.createElement("a");
    a.href = url;
    a.download = "beautified.js";
    document.body.appendChild(a);
    a.click();
    document.body.removeChild(a);
    URL.revokeObjectURL(url);
  }
</script>

<div class="container">
  <h1>JavaScript Beautifier</h1>

  <div class="input-section">
    <textarea
      bind:value={inputCode}
      placeholder="Paste your minified JavaScript code here..."
      rows="10"
    ></textarea>

    <div class="button-group">
      <button on:click={clearInput} class="clear">Clear</button>
      <button on:click={beautifyCode} class="beautify">Beautify</button>
    </div>
  </div>

  <div class="output-section">
    <textarea
      value={outputCode}
      placeholder="Beautified code will appear here..."
      rows="10"
      readonly
    ></textarea>

    <div class="button-group">
      <button on:click={copyToClipboard} class="copy">Copy to Clipboard</button>
      <button on:click={downloadCode} class="download">Download</button>
    </div>
  </div>
</div>

<style>
  .container {
    max-width: 1200px;
    margin: 0 auto;
    padding: 1rem;
  }

  h1 {
    text-align: center;
    color: #333;
    margin-bottom: 2rem;
  }

  .input-section,
  .output-section {
    margin-bottom: 2rem;
  }

  textarea {
    width: 100%;
    padding: 1rem;
    border: 1px solid #ccc;
    border-radius: 4px;
    font-family: monospace;
    font-size: 14px;
    resize: vertical;
    margin-bottom: 1rem;
  }

  .button-group {
    display: flex;
    gap: 1rem;
    justify-content: center;
  }

  button {
    padding: 0.75rem 1.5rem;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    font-weight: bold;
    transition: opacity 0.2s;
  }

  button:hover {
    opacity: 0.9;
  }

  .clear {
    background-color: #dc3545;
    color: white;
  }

  .beautify {
    background-color: #0d6efd;
    color: white;
  }

  .copy {
    background-color: #198754;
    color: white;
  }

  .download {
    background-color: #6c757d;
    color: white;
  }

  /* Responsive Design */
  @media (max-width: 768px) {
    .container {
      padding: 0.5rem;
    }

    textarea {
      padding: 0.75rem;
    }

    button {
      padding: 0.5rem 1rem;
      font-size: 14px;
    }

    .button-group {
      flex-direction: column;
    }

    button {
      width: 100%;
    }
  }
</style>
