# AutoEIT Transcription Technical Test - GSoC 2026

This repository contains my submission for Specific Test I: Audio File Transcription for the AutoEIT project as part of Google Summer of Code 2026.

The Engineering Approach
Rather than simply running a default transcription script, I conducted an iterative investigation into how ASR models such as OpenAI Whisper handle bilingual L2 learner data.

## Engineering Journey
Rather than submitting a static output, I utilized this test to investigate the failure points of baseline ASR models when applied to bilingual L2 learner data. My iterations revealed three critical challenges:
1. Hallucination Spirals: Whisper-base struggles with non-native Spanish phonetics and background artifacts, leading to looping or nonsensical output.
2. The Constraint Trap: Forcing a strict Spanish language lock (language="es") on bilingual audio (English instructions + Spanish responses) triggers logic collapses, resulting in phonetic and cross-lingual character hallucinations.
3. The 30-Second Trap: Whisper's auto-detection locks into the initial instructional language (English), causing the subsequent Spanish production to be translated or ignored.

## Proposed GSoC Architecture
To address these findings, I am proposing a transition from "Single-Pass Transcription" to a "Dynamic Pipeline." My full proposal will detail a two-stage solution:

- Stage 1: Adaptive Pre-Processing (VAD & Diarization)
    *The Logic:* Utilize Voice Activity Detection (VAD) and Speaker/Language Diarization to segment the audio into instructional (English) and participant (Spanish) chunks.
    *The Goal:* Programmatically strip instructional English speech prior to transcription. This prevents the "30-Second Auto-Detect Trap" and ensures the ASR engine remains anchored to the target language context.
  
- Stage 2: Intelligent Context Switching (Sliding Window Decoder)
    *The Logic:* Implement a sliding window decoder that monitors language probability scores (Logits) in real-time across a ±5 word contextual buffer.
    *Performance:* By utilizing a window-based approach, the system maintains Linear Time Complexity $O(n)$, ensuring that real-time language switching remains computationally efficient and scalable for long-form data without exponential growth in computational cost.


## 📂 Repository Structure
01_Audio_Transcription_Exploration.ipynb: Complete Jupyter Notebook containing the experimental logs, code iterations, and final data tables.

