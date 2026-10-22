# Last One Standing: live elimination quiz

One page, two views:

- **Host (big screen):** `your-site.netlify.app/?host`
- **Players (phones):** `your-site.netlify.app/?pin=123456` (the QR code on the big screen opens this)

Built for 300–400 people at once. Phones only send a tap; all the drama is on the big screen.

---

## Try it right now (demo mode, no setup)

Open `index.html?host` in a browser, create a game, then click **Open a test player tab** a few times.
Each tab is a separate player. Demo mode only works inside one browser, so use it to rehearse, not for the event.

## Set up for a real event (about 15 minutes, one time)

You already use Firebase + Netlify for iRace2Learn, so this will feel familiar.

1. **Firebase project.** Use your existing project or create a new one at console.firebase.google.com.
2. **Realtime Database.** Build → Realtime Database → Create database (choose the Singapore/asia-southeast1 region for India). Start in *locked mode*.
3. **Database rules.** Open the *Rules* tab, paste everything from `database.rules.json`, and click **Publish**.
4. **Anonymous sign-in.** Build → Authentication → Sign-in method → enable **Anonymous**. (Players never see a login; this just stops people tampering with the game.)
5. **Blaze plan (important for 400 people).** The free Spark plan caps the database at **100 simultaneous connections**. Upgrade the project to **Blaze** (pay-as-you-go). A 400-person game uses a tiny amount of data, so the cost is typically negligible, but set a budget alert to be safe.
6. **Web config.** Project settings → Your apps → Web app → copy the `firebaseConfig` object. Make sure it includes `databaseURL`.
7. **Paste the config** into `index.html` near the top, replacing `const FIREBASE_CONFIG = null;`:
   ```js
   const FIREBASE_CONFIG = {
     apiKey: "...",
     authDomain: "...",
     databaseURL: "https://....asia-southeast1.firebasedatabase.app",
     projectId: "...",
     appId: "..."
   };
   ```
8. **Deploy.** Drag the folder onto Netlify (app.netlify.com → Add new site → Deploy manually). Done.
9. **Authorised domain.** In Authentication → Settings → Authorized domains, make sure your Netlify domain is listed.

## Before the event

- Do a **dry run with 20–40 colleagues on their own phones** (not tabs on one laptop: Firebase gives every tab in one browser the same identity).
- Review the question set on the host setup screen (expand *Review…* to see answers).
- Ask the venue for Wi-Fi capacity; tell players **mobile data works fine**.
- Use the **Warm-up round** first so the room learns the flow in 2 minutes.

## Running the game

1. Open `/?host` on the laptop connected to the projector. Press ⛶ for full screen.
2. Pick a set → choose settings → **Create game**.
3. Players scan the QR code and type their name. Tap × on a name chip to remove a silly name.
4. Press **Start game**. Then just keep pressing **Space** (or the orange button): it reveals and moves to the next question.
5. At the end, **Download results (CSV)** gives every player's result for your records.

If the host laptop refreshes or crashes, open `/?host` again on the same browser and click **Resume it**.

### Settings

| Setting | What it does | Suggestion for 400 people |
|---|---|---|
| Seconds per question | Countdown length | 15s |
| End when players left ≤ | Stops the game once this few remain; ties broken by speed | 3 or 5 (gives you a podium) |
| Questions to use | Use all, or the first 10/15/20 | 15–20 |
| Shuffle order | Randomises question order | On |
| Reveal early | Reveals as soon as every survivor has answered | On (keeps pace up) |

400 players halve roughly every 1–2 questions on medium questions, so 15–20 questions is usually plenty.

## Adding your own questions

On the host screen click **+ Add your own set**, then either paste or upload a CSV.

**Paste format** (blank line between questions):

```
Which body regulates banks in India?
A) SEBI
B) RBI *
C) IRDAI
D) NABARD
Fact: Optional line shown after the reveal.
```

Mark the right answer with `*`, or add a line `Answer: B`. 2 to 4 options per question.

**CSV format** (download the template from the same screen):

```
question,option_a,option_b,option_c,option_d,answer,fact
```

Sets are saved in the host browser. Use **Copy & edit** on the built-in Current Affairs set to update it later.

## Rules (read these out, or press "Show rules" in the lobby)

1. Join on your phone by scanning the QR code or entering the game PIN. Use your real name.
2. Everyone starts **IN**. Each question has a countdown.
3. Answer correctly before time runs out to survive. A wrong answer or no answer means you're **OUT**.
4. **Mass save:** if every remaining player gets a question wrong, nobody is eliminated that round.
5. Out? Keep playing in **Crowd mode**. The big screen shows how the crowd did.
6. The game ends when only the target number of players are left, or the questions run out.
7. If several players are still standing, the **fastest total answer time** ranks them.

People who join after the game starts play in crowd mode for fun.

## Housekeeping

Each game is stored under `los/<PIN>` in the database. Delete old games from the Firebase console occasionally.
