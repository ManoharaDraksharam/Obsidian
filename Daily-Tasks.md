<%*
const folder = "Tasks";
const subfolder = "Daily-Tasks";
const baseFolder = `${folder}/${subfolder}`;

// ensure folders exist
if (!app.vault.getAbstractFileByPath(folder)) await app.vault.createFolder(folder);
if (!app.vault.getAbstractFileByPath(baseFolder)) await app.vault.createFolder(baseFolder);

// parse date from title; supports "2025-08-16-Tasks"
const title = tp.file.title;
const match = title.match(/(\d{4})-(\d{2})-(\d{2})/);
const currentDay = match
  ? moment(`${match[1]}-${match[2]}-${match[3]}`, "YYYY-MM-DD")
  : moment();

// folders by Year/Month
const yearFolder = `${baseFolder}/${currentDay.format("YYYY")}`;
const monthFolder = `${yearFolder}/${currentDay.format("MM")}`;
if (!app.vault.getAbstractFileByPath(yearFolder)) await app.vault.createFolder(yearFolder);
if (!app.vault.getAbstractFileByPath(monthFolder)) await app.vault.createFolder(monthFolder);

// move file into correct location
const filename = `${currentDay.format("YYYY-MM-DD")}-Tasks`;
const desired = `${monthFolder}/${filename}`;
if (tp.file.path !== `${desired}.md`) {
  await tp.file.move(desired);
}

// navigation
const prevDay = currentDay.clone().subtract(1, "day");
const nextDay = currentDay.clone().add(1, "day");

tR += `<< [[${baseFolder}/${prevDay.format("YYYY")}/${prevDay.format("MM")}/${prevDay.format("YYYY-MM-DD")}-Tasks|◀ Previous Day]] | [[${baseFolder}/${nextDay.format("YYYY")}/${nextDay.format("MM")}/${nextDay.format("YYYY-MM-DD")}-Tasks|Next Day ▶]] >>\n\n`;
%>

```tasks
not done 
due on <% currentDay.format("YYYY-MM-DD") %>
sort by due
```


### Completed  Tasks
```tasks
done 
due on <% currentDay.format("YYYY-MM-DD") %>
sort by due
```
