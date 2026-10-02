# Talk to Instinct

Record a voice memo to your AI assistant from your iPhone's side button, in one gesture.

Press the button, talk, tap to finish. The recording is sent as a voice memo to your assistant, which transcribes it and acts on it. No app, no server, no account beyond the ones you already have.

This repo documents the build. It is a pattern, not a product: the same few actions work with any assistant that can receive an iMessage, or any endpoint that accepts an HTTP POST.

## How it works

```
Action Button / side button  (or Siri: "Talk to Instinct")
        |
  Record Audio      Audio Quality: Normal
                    Start Recording: Immediately
                    Finish Recording: On Tap
        |
  Send Message      Recorded Audio -> your assistant, Show When Run off
```

Show When Run is off, so the message sends in the background. You never see a compose sheet. The assistant transcribes the voice memo when it arrives.

## Requirements

- iPhone with Shortcuts (iOS 17 or later for the Action Button on supported models; side-button double-tap or Back Tap work on others)
- An assistant reachable by iMessage or by webhook
- iMessage enabled on the phone (voice memos send as iMessage attachments)
- Microphone access for Shortcuts

## Quick start

See [BUILD.md](BUILD.md) for the step-by-step recipe. Short version:

1. Create a shortcut named after the Siri phrase you want.
2. Add **Record Audio**: Audio Quality **Normal**, Start Recording **Immediately**, Finish Recording **On Tap**.
3. Add **Send Message** with the Recorded Audio, to your assistant's number. Turn **Show When Run** off.
4. Bind it to the Action Button.

## Variants

### A. Voice memo by iMessage (zero infrastructure)

Send the recorded audio to your assistant's phone number or Apple ID. This is the default and needs nothing else. The assistant transcribes the memo on receipt.

The default recipient is Instinct's public line, +16504440920. If you use a different assistant, replace the recipient with its number or address.

**Simpler alternative: Dictate Text.** Replace Record Audio with **Dictate Text** (Stop Listening **After Pause**) and send the Dictated Text instead. This sends plain text with no audio file, so there is nothing to transcribe. The trade-off: iOS does the transcription on the phone, and you lose the original audio.

### B. POST to a webhook

For a fleet, router, or self-hosted agent, replace the Send Message step with **Get Contents of URL**:

- URL: your endpoint
- Method: POST
- Request Body: JSON
  - `text`: Dictated Text
  - `source`: `ios-shortcut`
  - `sent_at`: Current Date (ISO 8601)
- Headers: whatever your endpoint requires (for example `Authorization: Bearer <token>`)

This variant sends text, so it starts with **Dictate Text**, not Record Audio.

Keep the token out of anything you share. When you export the shortcut, remove it first or use the setup question below.

Retargeting later is a one-step edit. The button and the phrase stay the same.

## Let anyone retarget it

Shortcuts can ask for a value when someone adds it. In the shortcut's Import Questions (shortcut settings, Setup), add a question for the recipient, such as "Who should receive your voice notes? Enter a phone number or Apple ID." The answer fills the Send Message recipient. For the webhook variant, ask for the URL.

With that, one shared shortcut works for everyone and carries no one's number or token.

## Sharing the shortcut file

Signed `.shortcut` files can only be exported from a device. They cannot be authored from text or built in CI. So this repo documents the build, and the maintainer adds an iCloud share link here after building it on an iPhone:

> Shortcut link: https://www.icloud.com/shortcuts/ecd2cff174a74eba8c5b0fdde83174b4

Before exporting, check the Send Message recipient and any tokens. Use import questions so neither ships in the file.

## Notes and limits

- The default shortcut sends audio. The assistant transcribes it, so accuracy depends on the assistant's transcription and on recording quality. Short, clear notes work best.
- Dictate Text and the webhook variant send text, not audio.
- iMessage sends need the phone to be unlocked or to allow Shortcuts to run when locked. Test it with the screen off.
- Your assistant hears whatever you say. Pick a recipient you trust.
- Apple may change Shortcuts action names and settings between iOS versions.

## Contributing

Variants for other assistants, platforms (Android Tasker, macOS), and endpoints are welcome. Open an issue or a pull request.

## License

MIT licensed. See the included [LICENSE](LICENSE) file.
