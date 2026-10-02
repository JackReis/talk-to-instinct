# Build recipe

Two minutes in the Shortcuts app. Stock actions only.

## iMessage voice memo variant (default)

1. Open Shortcuts, tap **+**, and name the shortcut **Talk to Instinct**. The name is the Siri phrase, so pick what you want to say.
2. Add **Record Audio**. Set **Audio Quality** to **Normal**, **Start Recording** to **Immediately**, and **Finish Recording** to **On Tap**.
3. Add **Send Message**. Set the message to the **Recorded Audio** variable.
4. Tap the recipient and enter your assistant's number. The default is Instinct's public line, +16504440920.
5. Tap the arrow on Send Message and turn **Show When Run** off. This makes it send in the background.
6. Bind it:
   - Action Button models: Settings > Action Button > Shortcut > choose it.
   - Others: Settings > Accessibility > Touch > Back Tap, or set a side-button shortcut through Control Center.
7. Test: press the button, say "test note", tap to finish. Check the thread on the assistant side. The assistant transcribes the memo on receipt.

### Simpler alternative: Dictate Text

Use this if you want plain text with no audio file.

1. Step 1 as above.
2. Add **Dictate Text** instead of Record Audio. Set **Stop Listening** to **After Pause**.
3. Add **Send Message** with the **Dictated Text**, to the same recipient, **Show When Run** off.
4. Bind as in step 6.

## Webhook variant

Step 1 as above, then:

2. Add **Dictate Text**. Set **Stop Listening** to **After Pause**.
3. Add **Get Contents of URL**. Set the URL to your endpoint.
4. Method **POST**. Request Body **JSON**. Add `text` (Dictated Text), `source` (`ios-shortcut`) and `sent_at` (Current Date).
5. Add any auth header your endpoint needs.
6. Bind as in step 6 above.

## Add the setup question

1. Open the shortcut, tap its name, then **Setup**.
2. Add an import question tied to the recipient field (or the URL field), with a clear prompt.
3. Anyone who imports the shortcut is asked once and the answer is filled in.

## Export

1. Remove any personal number or token, or confirm import questions cover them.
2. Share > Copy iCloud Link.
3. Paste the link into README.md.

## Troubleshooting

- Nothing sends: confirm Show When Run is off, and that the recipient is reachable by iMessage.
- Voice memos need iMessage enabled for background send.
- Recording ends too soon or not at all: confirm Finish Recording is **On Tap**, and tap once when done.
- Dictation cuts off early (Dictate Text): try **On Tap** for stop listening.
- Button does nothing: re-select the shortcut in the Action Button setting.
