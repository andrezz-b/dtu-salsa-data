<%*
// 1. Get the file and current content
const file = tp.file.find_tfile(tp.file.path(true));
const rawContent = await app.vault.read(file);
const fm = tp.frontmatter;

// --- HELPER FUNCTION FOR LISTS ---
// Converts strings, arrays, or nulls into clean YAML lists
const formatList = (val) => {
    if (!val) return ""; // Returns empty string for null/undefined
    if (Array.isArray(val)) {
        if (val.length === 0) return "";
        // Filter out nulls and join
        return "\n" + val.filter(v => v).map(v => `  - "${v}"`).join("\n");
    }
    // Handle case where it was a single string value (not a list)
    return `\n  - "${val}"`;
};

// 2. Map Properties
const type = "partner";
const difficulty = fm.difficulty || fm.move_confidence || ""; 
const level = fm.level || "";
const leader_diff = fm.leader_difficulty || fm.leads_difficulty || "";
const follower_diff = fm.follower_difficulty || fm.follows_difficulty || "";

// 3. Process Lists using the helper
const aliases = formatList(fm.aliases);
const related_concepts = formatList(fm.related_concepts);
const related_moves = formatList(fm.related_moves);
const setup_moves = formatList(fm.setup_moves);
const exit_moves = formatList(fm.exit_moves);

// 4. Dates
const cDate = tp.file.creation_date("YYYY-MM-DD, H:mm");
const uDate = tp.file.last_modified_date("YYYY-MM-DD, H:mm");

// 5. Extract Old Body (Strip YAML)
const oldBody = rawContent.replace(/^---\n[\s\S]+?\n---\n?/, "").trim();

// 6. Construct NEW Content
const newFileContent = `---
type: ${type}
difficulty: ${difficulty}
level: ${level}
leader_difficulty: ${leader_diff}
follower_difficulty: ${follower_diff}
created_date: ${cDate}
updated_date: ${uDate}
tags:
  - salsa/move
aliases:${aliases}
related_concepts:${related_concepts}
related_moves:${related_moves}
setup_moves:${setup_moves}
exit_moves:${exit_moves}
---
## ✍️ Description & Execution
### Leader
- 
### Follower
- 
## 🎥 Video
- **Video Link:** 

# 🚧 MIGRATION ZONE (Sort and Delete)
> [!WARNING] Data Migration
> Move the content below into the sections above, then delete everything below this line.

${oldBody}
`;

// 7. Overwrite File
await app.vault.modify(file, newFileContent);
%>