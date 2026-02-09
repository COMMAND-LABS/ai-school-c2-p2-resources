# TLDR

Herein lies a collection of end-to-end workflows that enable you to generate news reports delivered by AI avatars.

## 3rd-party platforms

- n8n (https://n8n.io)
- Dropbox (https://www.dropbox.com)
- Dropbox Developer Console (https://www.dropbox.com/developers)
- HeyGen (https://www.heygen.com)
- ElevenLabs (https://elevenlabs.io)
- JSON2Video (https://json2video.com)
- Perplexity (https://www.perplexity.ai)
- OpenAI API (https://platform.openai.com)

## How to use

- 1. Research + Text-2-Speech
  - Performs research on a configurable topic via the Perplexity API
  - Research is then transformed into a structure news report via the OpenAI API
  - Each segment of the news report is converted into speech (text-2-speech) via the ElevenLabs API
  - Generated audio is stored in Dropbox for downstream use
- 2a. Generate AI Avatar Clips
  - Deliver your news report via a HeyGen avatar
- 2b. Generate AI Avatar Video (for retries)
  - Sometimes the HeyGen API will error out when generating your clips
  - This workflow enable you to retry the segments that fail
- 3a. Compile Final News Report
  - Compile all the generated media assets (video, script, audio) into the final news report via the JSON2Video API
- 3b. Test Titles
  - Little utility workflow for refining/experimenting with the "news-style" title overlays
- UTILITY: Generate Dropbox Share Links
  - Dropbox requires you to explicitly generate links for downloading files stored in your Dropbox account
  - This is will require a custom OAuth2 Client to get working

## Additional Documentation

- https://elevenlabs.io/app/voice-library?required_languages=fr&accent=creole
- https://elevenlabs.io/docs/api-reference/text-to-speech/convert
- https://elevenlabs.io/docs/api-reference/models/list
- https://developers.dropbox.com/oauth-guide
- https://docs.heygen.com/reference/create-an-avatar-video-v2
- https://app.heygen.com/avatars

## How to use this solution (high-level)

1. Research a topic (via Perplexity)
2. Write a news report script
3. TTS
4. UTILITY: Generate Dropbox "Share Links" (for script.json and audio files)
5. Generate AI Avatar videos via the HeyGen API (using TTS audio)
6. UTILITY: Generate Dropbox "Share Links" (for video)
7. Compile the assets into your final video (via JSON2Video)
