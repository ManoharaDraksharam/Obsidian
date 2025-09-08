
<%*
const folder = "Tasks";
const subfolder = "Monthly-Tasks";
const baseFolder = `${folder}/${subfolder}`;

// ensure folders exist
if (!app.vault.getAbstractFileByPath(folder)) await app.vault.createFolder(folder);
if (!app.vault.getAbstractFileByPath(baseFolder)) await app.vault.createFolder(baseFolder);

// parse date from title; supports "2025-08-Tasks"
const title = tp.file.title;
const match = title.match(/(\d{4})-(\d{2})/);
const currentMonth = match
  ? moment(`${match[1]}-${match[2]}`, "YYYY-MM")
  : moment();

// year folder
const yearFolder = `${baseFolder}/${currentMonth.format("YYYY")}`;
if (!app.vault.getAbstractFileByPath(yearFolder)) await app.vault.createFolder(yearFolder);

// move file into correct location
const filename = `${currentMonth.format("YYYY-MM")}-Tasks`;
const desired = `${yearFolder}/${filename}`;
if (tp.file.path !== `${desired}.md`) {
  await tp.file.move(desired);
}

// navigation
const prevMonth = currentMonth.clone().subtract(1, "month");
const nextMonth = currentMonth.clone().add(1, "month");

tR += `<< [[${baseFolder}/${prevMonth.format("YYYY")}/${prevMonth.format("YYYY-MM")}-Tasks|◀ Previous Month]] | [[${baseFolder}/${nextMonth.format("YYYY")}/${nextMonth.format("YYYY-MM")}-Tasks|Next Month ▶]] >>\n\n`;
%>
```tasks
not done
due on or after <% currentMonth.startOf("month").format("YYYY-MM-DD") %>
due on or before <% currentMonth.endOf("month").format("YYYY-MM-DD") %>
sort by due
```

### Completed Tasks (<% tp.file.title %>)

```tasks
done
due on or after <% currentMonth.startOf("month").format("YYYY-MM-DD") %>
due on or before <% currentMonth.endOf("month").format("YYYY-MM-DD") %>
sort by due
```