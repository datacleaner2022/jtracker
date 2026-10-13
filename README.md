# Job tracker in your browser (static edition)

Job tracker as one web page: no Python, nothing to install. Double-click `index.html` to open it.
Chrome and Edge work best; Firefox and Safari work too. The How to use guide (the **Guide** button) walks through it.

## What's different from the desktop app

- Everything you add is kept in this browser, on this computer: your tracker, resumes, documents,
  research and settings. Use the same browser each time. Clearing your browsing data clears Job tracker
  too, so download a backup now and then (**Settings**, **Local files**, **Back up everything**).
- Your tracker is the same Excel file as before. Open it with **Open a tracker file**.
  In Chrome and Edge, Job tracker stays linked to the file and saves every change straight into it,
  with its formats, dropdowns and formulas. In other browsers, a bar reminds you to download the
  updated tracker after making changes.
- Two ways to draft: free chat (copy and paste into ChatGPT or Claude) or a local model.
  A local model in LM Studio can also search the web for Find jobs (see below). API keys, the automatic
  job finder and the link check need the desktop app.
- Cover letters and resumes are made as Word files and PDFs and kept with each job. Download them,
  or use **Open folder** on a job to see everything kept for it.
- In Chrome and Edge you can also save your files to a folder on your computer: on the **Resume** tab,
  press **Choose folder**. Job tracker makes three folders in it: Job tracker files (saved there as
  you work), Custom resumes and Cover letters (each as a Word file and a PDF). The browser asks
  again on a later visit before Job tracker can write to it.
- The first time you open a spreadsheet, a Word file or a PDF, the page loads the tools it needs from
  a public code library (cdnjs.cloudflare.com and cdn.jsdelivr.net). Your files aren't sent there.

## Using a local model (Ollama or LM Studio)

A web page may only talk to a local model that allows it.

### Ollama

1. Quit Ollama.
2. Set the environment variable `OLLAMA_ORIGINS` to `*`, then start Ollama again.
   - Windows: `setx OLLAMA_ORIGINS "*"`
   - macOS: `launchctl setenv OLLAMA_ORIGINS "*"`
   - Linux: add `Environment="OLLAMA_ORIGINS=*"` to the ollama service, then restart it
3. In Job tracker: **Settings**, **Drafting with**, **Local model**. Server address `http://localhost:11434/v1`,
   the model you pulled (for example `llama3.1:8b`), then **Test connection**.

### LM Studio

1. In the Developer tab (the server), start the server and switch on **Enable CORS**.
2. In Job tracker: server address `http://localhost:1234/v1` and the model name, then **Test connection**.
   If LM Studio runs on another computer, use that computer's address, for example `http://192.168.2.132:1234/v1`.

### Web search for Find jobs (LM Studio 0.4 or newer)

A local model can't go online by itself. LM Studio can give it a web search plugin (an MCP server) that runs
on the computer where LM Studio is. Then **Find jobs**, **AI search** sends your search to the model, the
model searches the web, and the results come back in your tracker's columns.

Everything is done in LM Studio's own window, on the computer that runs it:

1. In a chat, open the **Program** tab on the right, then **Install**, **Edit mcp.json**, and add a web
   search server. Exa's hosted one needs nothing installed and no key (free, with a usage limit):

   ```json
   {
     "mcpServers": {
       "exa": {
         "url": "https://mcp.exa.ai/mcp"
       }
     }
   }
   ```

   If mcp.json already lists other servers, add the `"exa"` entry next to them.
2. In the Developer tab's **Server Settings**, switch on **Allow calling servers from mcp.json**.
3. Use a model that is good with tools (Qwen 3 or newer, gpt-oss, GLM 4.7 and the like).

The searches the model writes go to Exa; your resume stays with your model.

In Job tracker: **Settings**, **Local model**, **Web search plugin**: `exa` (the name from mcp.json).
Then in **Find jobs**, pick **AI search**. A search can take several minutes. Job tracker refuses results
when the model didn't search at all, and it removes any link that wasn't in the search results, so you
aren't sent to a page the model made up. Still check each posting before you apply.

### Opening the page from a local web address

Opening the page from a local web address instead of the file also works, and some browsers handle it
better. From this folder, run:

```
python -m http.server 8770
```

then go to http://localhost:8770/ (Ollama allows localhost pages without `OLLAMA_ORIGINS`).

## Privacy

Prompts for a chat website have your name, email, phone, address and links replaced with placeholders,
which are put back when you paste the reply. Nothing else leaves this page, except what you send to
your own local model.

---

This folder is built by `tools/sync_projects.py` in the Flask project, from the Django project's page.
Don't edit `index.html` by hand: change the sources and rebuild.
