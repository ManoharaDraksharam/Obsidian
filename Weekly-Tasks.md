

<%*
const folder = "Tasks";
const subfolder = "Weekly-Tasks";
const baseFolder = `${folder}/${subfolder}`;

// ensure folders exist
if (!app.vault.getAbstractFileByPath(folder)) await app.vault.createFolder(folder);
if (!app.vault.getAbstractFileByPath(baseFolder)) await app.vault.createFolder(baseFolder);

// parse week from title; supports "2025-W33-Tasks"
const title = tp.file.title;
const match = title.match(/(\d{4})-W(\d{2})/);
const currentWeek = match
  ? moment(`${match[1]}-W${match[2]}`, "GGGG-[W]WW") // GGGG for ISO week-year
  : moment();

// year folder
const yearFolder = `${baseFolder}/${currentWeek.format("GGGG")}`;
if (!app.vault.getAbstractFileByPath(yearFolder)) await app.vault.createFolder(yearFolder);

// move file into correct location
const filename = `${currentWeek.format("GGGG-[W]WW")}-Tasks`;
const desired = `${yearFolder}/${filename}`;
if (tp.file.path !== `${desired}.md`) {
  await tp.file.move(desired);
}

// navigation
const prevWeek = currentWeek.clone().subtract(1, "week");
const nextWeek = currentWeek.clone().add(1, "week");

tR += `<< [[${baseFolder}/${prevWeek.format("GGGG")}/${prevWeek.format("GGGG-[W]WW")}-Tasks|◀ Previous Week]] | [[${baseFolder}/${nextWeek.format("GGGG")}/${nextWeek.format("GGGG-[W]WW")}-Tasks|Next Week ▶]] >>\n\n`;
%>
```tasks
not done
due on or after <% currentWeek.startOf("isoWeek").format("YYYY-MM-DD") %>
due on or before <% currentWeek.endOf("isoWeek").format("YYYY-MM-DD") %>
sort by due
```


### Completed Tasks (<% tp.file.title %>)


```tasks
done
due on or after <% currentWeek.startOf("isoWeek").format("YYYY-MM-DD") %>
due on or before <% currentWeek.endOf("isoWeek").format("YYYY-MM-DD") %>
sort by due
```


