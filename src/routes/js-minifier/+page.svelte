<script lang="ts">
  import { onMount } from "svelte";

  let inputCode: string = "";
  let outputCode: string = "";

  // Function to minify JavaScript code
  function minifyCode() {
    if (!inputCode.trim()) return;

    try {
      // Basic minification rules
      outputCode = inputCode
        // Remove comments
        .replace(/\/\*[\s\S]*?\*\/|\/\/.*/g, "")
        // Remove whitespace between operators
        .replace(/\s*([+\-*/%=<>!&|,{}()[\];:?])\s*/g, "$1")
        // Remove unnecessary whitespace
        .replace(/\s+/g, " ")
        // Remove whitespace at start and end
        .trim();
    } catch (error: unknown) {
      if (error instanceof Error) {
        alert("Error minifying code: " + error.message);
      } else {
        alert("Error minifying code");
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

  // Function to download minified code
  function downloadCode() {
    if (!outputCode) return;

    const blob = new Blob([outputCode], { type: "text/javascript" });
    const url = URL.createObjectURL(blob);
    const a = document.createElement("a");
    a.href = url;
    a.download = "minified.js";
    document.body.appendChild(a);
    a.click();
    document.body.removeChild(a);
    URL.revokeObjectURL(url);
  }
</script>

<div class="w-full mx-auto p-4">
  <h1 class="text-center text-gray-800 text-3xl font-bold mb-8">
    JavaScript Minifier
  </h1>

  <div class="mb-8">
    <textarea
      bind:value={inputCode}
      placeholder="Paste your JavaScript code here..."
      rows="10"
      class="w-full p-4 border border-gray-300 rounded-lg font-mono text-sm resize-y mb-4"
    ></textarea>

    <div class="flex flex-col sm:flex-row justify-between gap-4">
      <button
        on:click={clearInput}
        class="bg-red-600 text-white font-bold py-3 px-6 rounded hover:opacity-90 transition-opacity"
      >
        Clear
      </button>
      <button
        on:click={minifyCode}
        class="bg-blue-600 text-white font-bold py-3 px-6 rounded hover:opacity-90 transition-opacity"
      >
        Minify
      </button>
    </div>
  </div>

  <div class="mb-8">
    <textarea
      value={outputCode}
      placeholder="Minified code will appear here..."
      rows="10"
      readonly
      class="w-full p-4 border border-gray-300 rounded-lg font-mono text-sm resize-y mb-4"
    ></textarea>

    <div class="flex flex-col sm:flex-row justify-between gap-4">
      <button
        on:click={copyToClipboard}
        class="bg-green-600 text-white font-bold py-3 px-6 rounded hover:opacity-90 transition-opacity"
      >
        Copy to Clipboard
      </button>
      <button
        on:click={downloadCode}
        class="bg-gray-600 text-white font-bold py-3 px-6 rounded hover:opacity-90 transition-opacity"
      >
        Download
      </button>
    </div>
  </div>
</div>
