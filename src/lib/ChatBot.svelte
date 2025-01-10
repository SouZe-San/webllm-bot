<script lang="ts">
  import {
    type ChatCompletionMessage,
    prebuiltAppConfig,
    CreateMLCEngine,
    MLCEngine,
  } from "@mlc-ai/web-llm";
  import { onMount } from "svelte";

  type messageType = { role: "system" | "user"; content: string };

  let isOpen = false;
  let prompt = "";
  let isModeLoad = false;
  let chatWaiting = false;
  let modelProgress: string;

  let messages: Array<messageType> = [
    {
      content:
        "You are a support chatbot on XYZ company. You are here to help the user with their queries.",
      role: "system",
    },
  ];

  let responses: Array<messageType | ChatCompletionMessage> = [
    {
      content: "Hi there! How can I assist you today?",
      role: "assistant",
    },
  ];

  async function toggleChat() {
    isOpen = !isOpen;
    if (!isModeLoad) {
      await onDownloadClick();
    }
  }

  // can change model as you want
  let SELECTED_MODEL = "Qwen2-0.5B-Instruct-q4f16_1-MLC";
  let engine: MLCEngine;

  function initProgressCallback(report: { progress: number; timeElapsed: number; text: string }) {
    modelProgress = report.text;
  }

  // Crate a engine instance
  onMount(async () => {
    engine = new MLCEngine();
    engine.setInitProgressCallback(initProgressCallback);
  });

  // Load Model from cache and load in GPU
  const onDownloadClick = async () => {
    if ((await engine.getGPUVendor()) === "") {
      modelProgress = "No GPU founded !!";
      return;
    }
    const config = {
      temperature: 1.0,
      top_p: 1,
    };
    try {
      await engine.reload(SELECTED_MODEL);
      isModeLoad = true;
      console.log("=============== All Done =====================");
    } catch (error) {
      console.log(error);
    }
  };

  // Function for send User prompt to LLM
  const handleSubmit = async (event: Event) => {
    event.preventDefault();
    if ((await engine.getGPUVendor()) === "") {
      return;
    }
    if (!isModeLoad) {
      return;
    }
    if (!engine) {
      console.error("MLCEngine not initialized.");
      return;
    }

    if (prompt.trim()) {
      chatWaiting = true;
      messages = [...messages, { content: prompt, role: "user" }];
      responses = [...responses, { role: "user", content: prompt }];
      prompt = "";

      try {
        const reply = await engine.chat.completions.create({
          messages: [...messages, { role: "user", content: prompt }],
        });

        // Update responses with the AI's reply
        responses = [...responses, reply.choices[0].message];
      } catch (error) {
        console.error("Error in chat completion", error);
      }
      chatWaiting = false;
    }
  };

  async function handleKeyPress(event: KeyboardEvent) {
    if (event.key === "Enter") {
      await handleSubmit(event);
    }
  }
</script>

<div class="chat-container">
  {#if isOpen}
    <div class="chat-window">
      <div class="chat-header">
        Support Chat
        <span>
          {modelProgress}
        </span>
      </div>
      <div class="chat-messages">
        {#each responses as message}
          <div class="message {message.role === 'user' ? 'user-message' : 'bot-message'}">
            <p class="chat-msg">{message.content}</p>
          </div>
        {/each}
        {#if chatWaiting}
          <div class="bot-thinking">typing...</div>
        {/if}
      </div>
      <div class="chat-input">
        <input
          type="text"
          placeholder="Type your message..."
          bind:value={prompt}
          on:keypress={handleKeyPress}
        />
        <button on:click={handleSubmit} disabled={!isModeLoad}>Send</button>
      </div>
    </div>
  {/if}

  <div class="bot-icon">
    <div class="hidden-text"><p>Ask Me Queries</p></div>
    <button
      class="botBtn"
      on:click={toggleChat}
      aria-label="Toggle chat"
      on:keypress={(event) => event.key === "Enter" && toggleChat()}
    >
      <svg xmlns="http://www.w3.org/2000/svg" width="128" height="128" viewBox="0 0 15 15"
        ><path fill="#ffffff" d="M7.5 5a1.5 1.5 0 1 0 0 3a1.5 1.5 0 0 0 0-3" /><path
          fill="#ffffff"
          fill-rule="evenodd"
          d="M9 2H8V0H7v2H6a6 6 0 0 0 0 12h3q.195 0 .389-.013l3.99.998a.5.5 0 0 0 .606-.606l-.577-2.309A6 6 0 0 0 9 2M5 6.5a2.5 2.5 0 1 1 5 0a2.5 2.5 0 0 1-5 0M7.5 12a4.48 4.48 0 0 1-2.813-.987l.626-.78c.599.48 1.359.767 2.187.767s1.588-.287 2.187-.767l.626.78A4.48 4.48 0 0 1 7.5 12"
          clip-rule="evenodd"
        /></svg
      >
    </button>
  </div>
</div>

<style>
  .bot-icon:hover {
    box-shadow: none;
    background: transparent;
    transition: background 0.3s;

    .hidden-text {
      display: flex;
      width: 14rem;
      justify-content: flex-start;
      align-items: center;
      padding: 10px;
      height: 80%;
      right: 3%;
    }
  }
  .chat-msg {
    margin: 0;
    padding: 0;
    display: inline;
  }
  .hidden-text {
    position: absolute;
    padding: 0;
    transition: all 0.3s ease-in-out;
    overflow: hidden;
    width: 0px;
    z-index: -1;
    border-radius: 10rem;
    background: linear-gradient(216deg, rgba(0, 206, 152, 1) 22%, rgba(0, 168, 192, 1) 71%);
    height: 0;
    right: 11.5%;
    border: 0;
    color: rgba(255, 255, 255, 0.837);
    p {
      margin-left: 1rem;
      font-size: 1rem;
      color: currentColor;
      font-style: oblique;
      font-weight: 700;
    }
  }

  .botBtn {
    background: transparent;
    border: none;
    outline: none;
    cursor: pointer;
  }
  @keyframes rotateScale {
    0% {
      transform: rotate(0deg) scale(1);
      color: transparent;
    }

    50% {
      transform: rotate(360deg) scale(1.5);
    }

    60% {
      transform: rotate(360deg) scale(1.5);
    }
    90% {
      color: rgba(255, 255, 255, 0);
    }
    100% {
      color: white;
      transform: rotate(0deg) scale(1);
    }
  }
</style>
