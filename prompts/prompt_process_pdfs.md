Process PDF files that have been placed in `publications/files_to_process/`.

For each PDF:

1. Read the first page to identify the publication (title, authors, journal/preprint server).
2. Determine whether it is a main paper PDF or supplementary information (SI).
3. Match it to a row in `backend/all_publications.csv` using the title and authors.
4. If the matched row does not yet have a `pub_id`, assign one using the convention below and add it to the CSV.
5. Create the directory `publications/files/{pub_id}/` if it does not exist.
6. Move the file there as `{pub_id}_main.pdf` or `{pub_id}_si.pdf` (do not copy — remove from `files_to_process/`).
7. Update the CSV: set `has_pdf` or `has_si` to `TRUE` for that row.
8. Delete any duplicate files (e.g., `file (1).pdf`) from `files_to_process/`.
8b. If a publication now has both `{pub_id}_main.pdf` and `{pub_id}_si.pdf`, combine them into `{pub_id}_all.pdf` using `pdfunite` (available at `/usr/local/bin/pdfunite`).

After organizing the files:

9. For each newly processed PDF where `led_by_kinney` is TRUE, read the full text (or at least the first and last pages, data availability section, and any code/software section) to look for:
   - **GitHub links** (e.g., github.com/...) — add to the `github` column if not already populated.
   - **ReadTheDocs links** (e.g., *.readthedocs.io) — add to the `readthedocs` column if not already populated.
   - **PyPI package names** that might imply a ReadTheDocs site — flag these for the user.
   - Only populate `github` and `readthedocs` for `led_by_kinney=TRUE` publications.
10. Report to the user what was processed, what links were found, and any issues.

Notes:
- PDFs and SI should be stored for ALL publications AND preprints (both led_by_kinney=TRUE and FALSE).
- GitHub and ReadTheDocs links should only be populated for `led_by_kinney=TRUE` entries.

Reference files:
- CSV: `backend/all_publications.csv`
- File storage: `publications/files/{pub_id}/{pub_id}_main.pdf` and `{pub_id}_si.pdf`
- pub_id convention: `FirstauthorYEAR_shortdesc` (e.g., `Kinney2010_sortseq`)
