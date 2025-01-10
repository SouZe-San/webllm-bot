# Lightweight Conversational AI with WebGPU 

## Overview

This project demonstrates an innovative architecture for deploying lightweight, optimized large language models (LLMs) in web applications. By leveraging the WebGPU web API, the solution enables efficient inference on client-side devices, promoting energy efficiency and sustainability in conversational AI. This demo is created for ICCRET 2025 paper submission.

## Features

- **Client-Side Inference**: Utilizes WebGPU for running models directly in the browser.
- **Optimized Model**: Implements the Qwen2.5-0.5B-Instruct-q4f16_1-MLC model for efficient performance.
- **User-Friendly Interface**: Built with Svelte.js for a responsive and intuitive user experience.

## Installation

To get started with this project, clone the repository and install the necessary dependencies:

```bash
git clone https://github.com/yourusername/your-repo-name.git
cd your-repo-name
bun install
```

## Usage

Run the application locally:

```bash
bun run dev
```

Open your browser and navigate to `http://localhost:5173` to access the demo customer support site.

## Acknowledgements

We would like to express our gratitude to the use of the WebLLM JavaScript Library, which facilitated the integration of pre-compiled models and loading to WebGPU.
