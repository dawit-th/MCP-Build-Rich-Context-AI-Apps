 # MCP-Powered Voice Agent with DeepSeek Integration

## Overview

This repository contains a comprehensive **MCP-Powered Voice Agent with DeepSeek Integration**. It's a sophisticated AI voice assistant that demonstrates the implementation of the Model Context Protocol (MCP) for structured, context-rich AI interactions. The system provides a complete, real-time voice processing pipeline within an interactive Jupyter notebook environment, designed for the research and development of advanced voice-based AI assistants.

## Key Features

-   **Comprehensive Documentation**: Every function and class is thoroughly documented with its purpose, parameters, and implementation details.
-   **Enhanced MCP Protocol**: Implements structured request/response handling, intelligent context compression, session state tracking, and performance monitoring.
-   **Advanced DeepSeek Integration**: Features a multi-tier response generation system (API primary with intelligent fallbacks), context-aware processing, and cost tracking.
-   **Robust Error Handling**: Ensures graceful degradation, with intelligent fallbacks and comprehensive exception management to maintain system stability.
-   **Performance Optimization**: Includes real-time metrics for processing time, memory management via context compression, and resource monitoring.

## System Architecture

The system is built on a linear pipeline architecture, ensuring a clear and sequential flow of data from voice input to voice output.

```mermaid
graph TB
    subgraph "User Interface Layer"
        UI[Jupyter Notebook Interface]
        Widgets[Interactive Widgets]
        Chat[Chat Interface]
    end
    
    subgraph "Audio Processing Layer"
        Mic[Microphone Input]
        SR[Speech Recognition]
        TTS[Text-to-Speech]
        Speaker[Audio Output]
    end
    
    subgraph "Core Processing Layer"
        Config[Enhanced Configuration]
        MCP[Enhanced MCP Protocol]
        DeepSeek[DeepSeek Integration]
        Context[Context Management]
    end
    
    subgraph "AI Model Layer"
        API[DeepSeek API]
        Local[Local Models]
        Fallback[Intelligent Fallbacks]
    end
    
    subgraph "Data Storage Layer"
        Session[Session State]
        History[Conversation History]
        Metrics[Performance Metrics]
        Cache[Response Cache]
    end
    
    %% Flow
    UI --> Mic --> SR --> MCP --> Context --> DeepSeek
    DeepSeek --> API & Local & Fallback
    API --> DeepSeek --> MCP --> TTS --> Speaker --> UI
    Context --> History & Session
    DeepSeek --> Metrics
    MCP --> Cache
```

## Technology Stack

-   **Runtime Environment**: Python 3.11 in Replit cloud platform.
-   **Development Platform**: Jupyter Notebook with `ipywidgets` for an interactive UI.
-   **Audio Processing**: `SpeechRecognition`, `pyttsx3`, `espeak-ng`, and `portaudio`.
-   **AI Processing**: `DeepSeek` (primary), `OpenAI` (fallback), `transformers`, and `torch`.
-   **Protocol Layer**: A custom implementation of the Model Context Protocol (MCP).

## Workflow and Data Flow

The agent follows a structured workflow to process voice interactions, from initial audio capture to the final voice response.

### Data Flow

The following diagram illustrates the step-by-step data flow, including validation, context management, and fallback logic.

```mermaid
flowchart TD
    Start([User Voice Input]) --> Capture[Audio Capture]
    Capture --> Recognition[Speech Recognition]
    Recognition --> Validation{Input Valid?}
    
    Validation -->|No| ErrorHandle[Error Handling]
    ErrorHandle --> FallbackResponse[Generate Fallback Response]
    
    Validation -->|Yes| MCPRequest[Create MCP Request]
    MCPRequest --> ContextGather[Gather Context]
    ContextGather --> TokenCheck{Token Limit Check}
    
    TokenCheck -->|Exceeded| Compress[Context Compression]
    Compress --> OptimizedContext[Optimized Context]
    TokenCheck -->|OK| OptimizedContext
    
    OptimizedContext --> APICheck{API Available?}
    
    APICheck -->|Yes| APICall[DeepSeek API Call]
    APICall --> APIResponse[Process API Response]
    
    APICheck -->|No| LocalModel[Local Model Processing]
    LocalModel --> LocalResponse[Process Local Response]
    
    APIResponse --> QualityCheck[Quality Analysis]
    LocalResponse --> QualityCheck
    FallbackResponse --> QualityCheck
    
    QualityCheck --> UpdateContext[Update Conversation Context]
    UpdateContext --> UpdateMetrics[Update Performance Metrics]
    UpdateMetrics --> MCPResponse[Create MCP Response]
    
    MCPResponse --> TextToSpeech[Text-to-Speech Conversion]
    TextToSpeech --> AudioOutput[Audio Output]
    AudioOutput --> End([Response Delivered])
```

### Component Interactions

This sequence diagram shows how the different components of the system interact during a single conversational turn.

```mermaid
sequenceDiagram
    participant User
    participant UI as Jupyter Interface
    participant Audio as Audio System
    participant MCP as MCP Protocol
    participant DeepSeek as DeepSeek Integration
    participant API as DeepSeek API
    participant Context as Context Manager
    
    User->>UI: Voice Input
    UI->>Audio: Capture Audio
    Audio->>Audio: Speech Recognition
    Audio->>MCP: Text Input
    
    MCP->>Context: Gather Context
    Context-->>MCP: Conversation History
    
    MCP->>MCP: Create Structured Request
    MCP->>DeepSeek: Process Request
    
    DeepSeek->>API: API Call (if available)
    alt API Available
        API-->>DeepSeek: AI Response
    else API Unavailable
        DeepSeek->>DeepSeek: Generate Fallback
    end
    
    DeepSeek-->>MCP: Generated Response
    
    MCP->>Context: Update Conversation
    
    MCP-->>Audio: Response Text
    Audio->>Audio: Text-to-Speech
    Audio->>UI: Audio Output
    UI->>User: Voice Response
```

## Getting Started

### Prerequisites

-   Python 3.11
-   System dependencies: `espeak-ng` and `portaudio`. These can typically be installed with a package manager (e.g., `sudo apt-get install espeak-ng portaudio19-dev` on Debian/Ubuntu).

### Installation

1.  **Clone the repository:**
    ```bash
    git clone https
