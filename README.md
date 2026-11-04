# MyGameList


A free game tracker that lives in **one file** (`index.html`).

Log what you play, rate and review games, keep a backlog, see upcoming releases, play minigames, and export everything to Excel.

No account, no install, no server. Your data stays in your own browser.


---


## Quick start (30 seconds)


1. Download `index.html`.

2. Double-click it. It opens in your browser. Done.


To put it online, drag `index.html` onto any static host, for example [Netlify Drop](https://app.netlify.com/drop) or GitHub Pages. Nothing else is needed.


> Your games are saved in the browser you use. Use **Settings > Backup & Restore** every now and then to save a backup file or even export it into an Excel sheet.


---


## What works with no additional setup


- Game list, reviews, backlog
- Play calendar and profile
- Custom Design settings, dark mode, holiday themes
- Excel export and backups
- News
- Search, covers and the monthly releases tracker, all 3 through Wikipedia


---


## Optional extras


Open `index.html` in any text editor (Notepad is fine).

At the top you will find:


```js
const SETTINGS = {
  RAWG_KEY: "",
  WORKER_URL: "",
};
```


Paste your key and/or Worker URL **between the quotes in the index file**, save, and refresh/load the website.

That is the only edit you should ever need in the file itself.

Below are the steps needed to get said key and URL

---


### 1. RAWG key (more games, ratings, playtime)


**IMPORTANT NOTE:** While RAWG.io is coded for the website, the website was designed for IGDB mostly. I would only use RAWG if you want access to the browse mode without having to setup a Worker URL.

Also, as of currently writing this, RAWG has been down for a few days. So this step isn't even possible right now lol.


1. Make a free account at https://rawg.io/apidocs and copy your API key.

2. Paste it in `RAWG_KEY: "your-key-here",` on the index.html file.


---


### 2. Worker URL (IGDB covers, release calendar, Browse filters, minigames, Sync)


The Worker is a small free program on Cloudflare that talks to IGDB for the site (IGDB needs secret keys, so the page cannot do it alone).

Without it, those features hide themselves or fall back politely.


#### Step-by-step: the Worker


1. Go to https://dev.twitch.tv/console and log in.

   Go to **Applications** on the sidebar and click **Register Your Application**.

   Use any name, OAuth redirect `http://localhost`, and category "Website Integration".

   Copy the **Client ID** and make a **Client Secret**, both for use in step 3.


2. Make a free account at https://dash.cloudflare.com

   Go to **Compute > Workers & Pages > Create Application > "Start with Hello World"**.

   Name it **mygamelist-covers** and click **Deploy**.

   Then click **Edit code**, delete what is there, paste in all of `cover-worker.js`, and **Deploy** again.


3. In the Worker, open **Settings > Variables and Secrets** and add two Variables:

   - Name one `TWITCH_CLIENT_ID`
   - Name the other `TWITCH_CLIENT_SECRET`

   For the values, copy the Client ID and Client Secret from step 1 and apply them to the variable of the same name.

   Click the secret button as well when making them.

   Then click "Add variable and deploy".


4. Copy your Worker URL from the overview page of "Workers & Pages". It looks like `mygamelist-covers.accountname.workers.dev`.

   Paste it in the `WORKER_URL: "",` line (no slash at the end) in the index.html file.

   From here, your Minigames, Monthly Releases, and Search/Browse should function exactly the way my site does.


#### Optional but helpful: enable Syncing between different devices


5. On the Cloudflare Dashboard go to **Storage & Databases > Workers KV** and create a namespace naming it mygamelist-sync.

6. Then in the Worker you made above, open **Settings > Bindings > Add binding > KV namespace**.

   Set the variable name to exactly `SYNC` (including the capitalization), pick your namespace from the dropdown, and click Add binding.

7. Since you should have already copied the Worker URL into the index, everything should just work automatically.


**Assuming everything was done correctly, adding all the optional settings from up above should add full functionality to the entire site!**

Cloudflare's free plan is plenty for personal use.


---


## Troubleshooting: "I pasted my Worker address and nothing changed"


1. Scroll to the very bottom of the website. When a Worker address is set, the footer shows one line:

   - `IGDB Worker: connected ✓` means it works.
   - `can't reach ...` means the address is wrong or the Worker is not deployed.
   - `answered with error ...` means the Worker runs but the `TWITCH_CLIENT_ID` / `TWITCH_CLIENT_SECRET` secrets are missing or wrong.


3. Sync in the Settings menu only works when a Worker URL is set. Release lists and search results you already viewed are remembered for a few hours, so give them a refresh.


4. Search page switches you toggled earlier are remembered by your browser. Tick IGDB on the Search page if it is off.


---


## Files


- `index.html`: the whole site.
- `cover-worker.js`: the optional Cloudflare Worker.
- `README.md`: you're currently reading what's in it.


---


## Privacy and safety


- Your list lives only in your browser's storage (and in files you export). Nothing is sent anywhere unless you set up Sync.

- Search and cover lookups send the game title you typed to Wikipedia/Wikidata, RAWG (if you added a key) and your own Worker (if you set one).

- Never put secrets other than the RAWG key in `index.html`. The Twitch keys belong only in Cloudflare's Secrets.


---


## Credits


Game data and images come from these services. Their logos are in the site footer so people know where things come from:


- [IGDB](https://www.igdb.com) (via Twitch): covers, search, release dates, ratings, game database
- [RAWG](https://rawg.io): game database, ratings, playtime. IGDB takes priority if you have both.
- [Wikipedia](https://www.wikipedia.org) / Wikidata: fallback search, covers and dates


All game names, covers and logos belong to their owners. This project is not affiliated with any of them.
