<%*
// 1. Get the file and current content
const file = tp.file.find_tfile(tp.file.path(true));
const rawContent = await app.vault.read(file);
const fm = tp.frontmatter;

// --- HELPER FUNCTION FOR LISTS ---
const formatList = (val) => {
    if (!val) return ""; 
    if (Array.isArray(val)) {
        if (val.length === 0) return "";
        return "\n" + val.filter(v => v).map(v => `  - "${v}"`).join("\n");
    }
    return `\n  - "${val}"`;
};

// 2. Map Properties
const type = fm.type || "";
const level = fm.level || "";

// 3. Process Lists
const aliases = formatList(fm.aliases);
const related_concepts = formatList(fm.related_concepts);
const related_moves = formatList(fm.related_moves);

// 4. Dates
const cDate = tp.file.creation_date("YYYY-MM-DD, H:mm");
const uDate = tp.file.last_modified_date("YYYY-MM-DD, H:mm");

// 5. Extract Old Body (Strip YAML)
const oldBody = rawContent.replace(/^---\n[\s\S]+?\n---\n?/, "").trim();

// 6. Construct NEW Content
const newFileContent = `---
type: ${type}
level: ${level}
created_date: ${cDate}
updated_date: ${uDate}
tags:
  - salsa/concept
aliases:${aliases}
related_concepts:${related_concepts}
related_moves:${related_moves}
---
## ✍️ Description
- 
## 🎥 Media
- **Video Link:** 
# 🚧 MIGRATION ZONE (Sort and Delete)
> [!WARNING] Data Migration
> Move the content below into the sections above, then delete everything below this line.

${oldBody}
`;

// 7. Overwrite File
await app.vault.modify(file, newFileContent);
%>