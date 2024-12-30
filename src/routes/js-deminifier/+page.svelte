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

<div class="max-w-7xl mx-auto p-4">
  <h1 class="text-center text-gray-800 mb-8 text-2xl font-bold">JavaScript Beautifier</h1>

  <div class="mb-8">
    <textarea
      bind:value={inputCode}
      placeholder="Paste your minified JavaScript code here..."
      rows="10"
      class="w-full p-4 border border-gray-300 rounded-md font-mono text-sm resize-y mb-4"
    ></textarea>

    <div class="flex justify-center gap-4 md:flex-row flex-col">
      <button on:click={clearInput} class="bg-red-600 text-white font-bold py-3 px-6 rounded hover:opacity-90 transition-opacity">Clear</button>
      <button on:click={beautifyCode} class="bg-blue-600 text-white font-bold py-3 px-6 rounded hover:opacity-90 transition-opacity">Beautify</button>
    </div>
  </div>

  <div class="mb-8">
    <textarea
      value={outputCode}
      placeholder="Beautified code will appear here..."
      rows="10"
      readonly
      class="w-full p-4 border border-gray-300 rounded-md font-mono text-sm resize-y mb-4"
    ></textarea>

    <div class="flex justify-center gap-4 md:flex-row flex-col">
      <button on:click={copyToClipboard} class="bg-green-600 text-white font-bold py-3 px-6 rounded hover:opacity-90 transition-opacity">Copy to Clipboard</button>
      <button on:click={downloadCode} class="bg-gray-600 text-white font-bold py-3 px-6 rounded hover:opacity-90 transition-opacity">Download</button>
    </div>
  </div>
</div>
