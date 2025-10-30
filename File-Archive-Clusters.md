# Daily Archive Filename Analysis – Technical Summary

## 1. Goal
The project aimed to analyze and summarize filename changes over time in the `./daily-archive` directory. Each daily JSON file (`filelist-YYYY-MM-DD.json`) contained metadata for multiple files.  

Key objectives:
1. Identify files updated daily or almost daily.
2. Detect related groups of files by name patterns or extensions.
3. Find files modified close together in time (within ~60 minutes).
4. Identify calendar gaps where no daily snapshot exists.

---

## 2. Parsing Daily Archives
- Python script loaded all `filelist-YYYY-MM-DD.json` files.
- Extracted filenames and modification timestamps.
- Constructed a **timeline of file changes** to track additions, deletions, and updates.
- Detected missing dates to highlight **calendar gaps**.

---

## 3. Export of Unique Filenames
- All unique filenames were exported to `unique_filenames.txt`.
- This file became the input for fuzzy string clustering and pattern analysis on another machine (Google Colab).

---

## 4. Fuzzy Clustering of Filenames
- **RapidFuzz** library used for fuzzy matching (Levenshtein-based similarity).
- Filenames were grouped if similarity ratio exceeded a threshold (`SIMILARITY_THRESHOLD = 85`).
- Clustering was refined to exclude irrelevant files.

### Filters applied:
- **Extensions skipped**: `.pdf`, `.jpg`, `.jpeg`, `.png`, `.gif`, `.bmp`, `.svg`, `.webp`, `.ico`.
- **Filename patterns skipped**: `filelist-`, `zvg_id`.
- **Minimum cluster size**: `MIN_CLUSTER_SIZE = 5`.

---

## 5. Longest Common Substring (LCS)
- For each cluster, the **longest common substring** was computed to summarize the shared naming pattern.
- Example:
```
Cluster 1 (LCS: 'project-report-v') → 12 files
project-report-v1.docx
project-report-v2.docx
project-report-v3.docx
...
```
---

## 6. Cluster Results
- Total clusters after filtering: **75**.
- Each cluster represents a meaningful group of related files or versions.
- LCS highlights the underlying naming patterns for easier interpretation.

---

## 7. Export to CSV
- All clusters were exported to `filename_clusters.csv` with columns:
- `cluster_index`
- `lcs` (shared substring)
- `num_files` (number of files in cluster)
- `file_list` (semicolon-separated filenames)

This allows easy inspection in Excel, Google Sheets, or other analysis tools.

---

## 8. Summary
- Large historical dataset (~5000 filenames) reduced to **75 meaningful clusters**.
- Filters and clustering removed irrelevant files and noise.
- Provides a clear overview of frequent updates, related files, and naming patterns.
- Workflow is repeatable and can be extended for future archive analysis or visualization.

---

**End of Report**

