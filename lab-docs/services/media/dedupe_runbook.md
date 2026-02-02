# Runbook: Library Deduplication & Root Cause Analysis

## Phase 1: Identification & Deduplication
- Movies: Utilize Radarr API/Bulk Edit to identify quality overlaps.
- Plex Filter: Use the 'Duplicates' filter in the Movie library to verify file paths.

## Phase 2: Root Cause Investigation
- Recycle Bin Check: Verify if Synology #recycle folders are being indexed.
- Mount Point Ghosting: Audit NFS/SMB mount history on Z240.
- Arr Logic: Check "Ignore Deleted" and "Recycle Bin" settings in Radarr/Sonarr.

## Phase 3: Prevention
- Update hardlink enforcement to prevent future duplicates.
