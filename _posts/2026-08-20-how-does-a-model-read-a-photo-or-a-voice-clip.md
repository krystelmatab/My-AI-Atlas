---
title: "How Does a Model Actually 'Read' a Photo or a Voice Clip?"
date: 2026-08-20
excerpt: "An interactive walkthrough of how text, images, audio, and video all get turned into the same kind of thing before a model can understand any of them — and where base64 actually fits in."
---

## 1. Introduction: The Word "Multimodal" Hides a Real Question

Every time you drop a photo into a chat with an AI model, something happens that's easy to wave away with one word: "multimodal." But what is actually going on? The model didn't grow eyes. It doesn't have ears. So what does it mean, mechanically, for a language model to "look at" a picture or "listen to" a voice clip?

The short answer: it doesn't, not in any sense a person would recognize. Everything — a sentence, a photo, a sound — gets translated into the same kind of object before the model ever touches it: a long list of numbers. This article walks through exactly how that translation happens, modality by modality, with an interactive piece you can click through yourself.

## 2. The Core Idea: One Shared Language

Text gets cut into small chunks by a tokenizer. Images get sliced into square tiles. Audio gets turned into a spectrogram — essentially a picture of sound — and sliced the same way. Video is treated as a stack of sampled frames, tiled like images, then compressed to keep the token count manageable.

Different starting materials, same destination: every one of them ends up as same-shaped vectors, lined up in a single sequence, that a transformer processes without ever knowing which modality a given position came from.

> The transformer has no separate "vision mode" or "audio mode" — it treats every position in the sequence the same way, whether it came from a tile of a photo or a word.

## 3. Try It Yourself

Reading about tokenization only goes so far — so I built an interactive piece instead. Click through each modality's tab, split an image into patches, turn a waveform into a spectrogram, and watch a live base64 encoder run on text you type.

<iframe src="{{ '/assets/interactive/multimodal-tokenization.html' | relative_url }}" width="100%" height="950" style="border:none;border-radius:12px;" loading="lazy"></iframe>

## 4. The Base64 Mix-Up: Packaging, Not Understanding

This is the part that trips people up most, myself included when I first looked into it. Images and audio really do get sent as base64 text — but that's a separate, earlier step, not how the model "reads" them.

Base64 exists because raw file bytes aren't safe to drop into a text-based message like a JSON request. So the bytes get repacked into plain text characters — the digital equivalent of melting a glass sculpture down into flat, mailable letters, then rebuilding it at the other end. The moment that base64 text arrives, it's decoded straight back into the original bytes. Only then does the real tokenization pipeline — the tiling, the spectrogram — begin.

The whole idea in one sentence: the file leaves as bytes, disguises itself as text so it can travel, travels, takes the disguise off when it arrives — and only then does the process of actually "understanding" it begin.

## 5. Why This Is Worth Knowing

This isn't just trivia. It explains real, observable behavior:

- Why models are sharper describing one photo in detail than tracking fast motion across a video — most of the in-between frames get thrown away before the model ever sees them.
- Why a model can miscount objects in a busy image — a patch summarizes a small region rather than preserving every pixel exactly.
- Why "sending a bigger image" or "a longer clip" isn't free — every tile and every audio slice is a token, and token budgets are finite.

## 6. Conclusion: No Eyes, No Ears, Just Numbers

There's no picture "behind" the tokens that a model looks at, the way there's a picture behind your own eyes when you look at a photo. The numbers are all it has — trained, through exposure to enormous amounts of paired data, to place related numbers close together regardless of which modality produced them. Understanding that boundary is the difference between finding a model's limitations mysterious and finding them predictable.
