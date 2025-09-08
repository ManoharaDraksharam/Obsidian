<%*
const folder = "Tasks";
const subfolder = "Yearly";
const baseFolder = `${folder}/${subfolder}`;

// ensure folders exist
if (!app.vault.getAbstractFileByPath(folder)) await app.vault.createFolder(folder);
if (!app.vault.getAbstractFileByPath(baseFolder)) await app.vault.createFolder(baseFolder);

// get year from title; works for "2025-Tasks" or just "2025"
const title = tp.file.title;
const match = title.match(/\b(\d{4})\b/);
const year = match ? parseInt(match[1], 10) : parseInt(tp.date.now("YYYY"), 10);

// move into correct folder/name if needed
const desired = `${baseFolder}/${year}-Tasks`;
if (tp.file.path !== `${desired}.md`) {
  await tp.file.move(desired);
}

// compute nav targets
const prevYear = (year - 1).toString();
const nextYear = (year + 1).toString();
%>

<< [[<% `${baseFolder}/${prevYear}-Tasks` %>|◀ Previous Year]] | [[<% `${baseFolder}/${nextYear}-Tasks` %>|Next Year ▶]] >>

```tasks
not done
due on or after <% moment(String(year), 'YYYY').startOf('year').format('YYYY-MM-DD') %>
due on or before <% moment(String(year), 'YYYY').endOf('year').format('YYYY-MM-DD') %>
sort by due
```


### Completed Tasks

```tasks
done
due on or after <% moment(String(year), 'YYYY').startOf('year').format('YYYY-MM-DD') %>
due on or before <% moment(String(year), 'YYYY').endOf('year').format('YYYY-MM-DD') %>
sort by due
```