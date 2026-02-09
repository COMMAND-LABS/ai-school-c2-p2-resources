# TLDR

Script outline for the `Build AI Avatar News Reports` YouTube walkthrough

## ToC

- PART 1: Research + Text-2-Speech
- PART 2: Generate AI Avatar Videos (the longest part)
- PART 3: Compile Final News Report
- Utility Workflows

## PART 1: Tips regarding the "Research + Text-2-Speech" workflow

- Check out the config nodes

- You'll need a Perplexity API key (ie: will perform research and provide references to where it's output is coming from)
  - https://www.perplexity.ai/
  - https://www.perplexity.ai/account/api/keys

- Notice the "Switch" node that enables us to generate the news report in multiple languages
  - We'll walk through generating the news report in English and, at the end of this walkthrough, generate it in French for fun
  - SIDENOTE: If you are culturally tuned in, then you know that the predominant language spoken in AYITI is Kreyol. I tried to find an AI model specially trained for speaking Kreyol but didn't find one so we are going to go with French.

- You'll need an OpenAI API key (ie: for writing the news report script)
  - https://platform.openai.com
  - https://platform.openai.com/api-keys
  - NOTE: we are using `GPT-5.1`

- TTS with the ElevenLabs API for each segment of the news report
  - Browse the available voices on ElevenLabs here: https://elevenlabs.io/app/voice-library
    - The voice ID is what is used in the workflow
      - 6aDn1KB0hjpdcocrUkmq (ie: "Tiffany" for the English delivery)
      - McVZB9hVxVSk3Equu8EH (ie: "Audrey" for the French delivery)
  - PRO TIP: Verify generated audio "sounds good" before continuing in order to not waste time
  - https://elevenlabs.io
  - https://elevenlabs.io/app/developers/api-keys
  - https://elevenlabs.io/docs/api-reference/text-to-speech/convert
  - https://elevenlabs.io/docs/api-reference/models/list

- Upload the generated assets (audio and script) to Dropbox for use in later workflows (which we'll take a look at shortly)
  - https://www.dropbox.com

- IMPORTANT: Here use the utility `Generate Dropbox Share Links` workflow

## PART 2a - Generate AI Avatar Video (1 scene)

- If you'd like to generate the AI Avatar videos 1 scene at a time to not burn credits use this workflow
- NOTE: The use of a Data Table for storing the location of the media assets we are working with
- NOTE: How the HeyGen Avatar ID is configurable
  - https://app.heygen.com/avatars?tab=public
- See how we're looping over all the media assets in the provided Dropbox folder
- If we see an .mp3 file (which was generated using ElevenLabs)...
  - Then we build a "downloadable file" share link
  - As there is no built-in HeyGen node in n8n, we must build an HTTP Request to send to the HeyGen API ourselves...
    - https://docs.heygen.com/reference/create-an-avatar-video-v2

## PART 2b - Generate AI Avatar Clips (all scenes)

- NOTE: How only n of the 3 generation requests succeeded
- 2a is a workflow for retrying the failed generations
- NOTE: I know this is the slowest part of this process
- If anyone listening knows of an alternative API to the HeyGen API for making AI Avatar videos please leave a comment so I can check it out
  - The overall workflow being presented in this walkthrough can be tweaked to plug into any API
- Let's check out what was generated
  - Once you have a good generation for each segment of the news report, download each clip and upload it to the Dropbox folder we are working in

- IMPORTANT: Here use the utility `Generate Dropbox Share Links` workflow

## PART 3a - Compile Final News Report

- In the "Compile Final News Report" workflow, we will loop over all the files in our Dropbox folder and group them based on scene
- In order to render the final video we will use an API called JSON2Video. There are free alternatives but for simplicity we will stick with JSON2Video
- Send to JSON2Video API

## PART 3b - Test Titles

- Workflow 3b is for experimenting with tweaking the "news-style" overlay titles

## Utility Workflows

### "Generate Dropbox Share Links" workflow

- For data security reasons, Dropbox does NOT automatically make uploaded files accessible via a URL
- So we'll have to create a "Share Link" for each file we'll be accessing downstream
  - Let me show you what I mean (Go to Dropbox to show how to generate a "Share Link")
- At the time of recording, the built-in OAuth2 client that comes with n8n has support for these permissions when interacting with your Dropbox account...
  - files.content.write
  - files.content.read
  - sharing.read
  - account_info.read

- So we'll need to create a custom OAuth2 client with an additional permission for creating "Share Links"
  - ie: the `sharing.write` permission

- This workflow loops through all the files we just uploaded to Dropbox and generates a public "Share Link" for them so we can use them downstream
  - NOTE: The use of a Data Table for sharing state across the workflows
    ie: we storing the location in Dropbox where the media assets we are working with are located
  - NOTE: the subtle error handling we are doing here
    - If a file already has a "Share Link" generated that is OK (ie: don't throw an error) just continue generating the Share Links for the remaining files
