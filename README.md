NEXUS AI — Online Content Intelligence Engineer

Overview

The Online Content Intelligence module is responsible for importing educational resources, processing lecture transcripts, indexing timestamped content, enabling video search, and integrating with the existing NEXUS AI learning platform.

NEXUS AI is an educational platform that combines a React frontend, Node.js backend, and Python AI/RAG service to help students access and search educational content.

Implementation scope: Online functionality only. Offline functionality is currently out of scope.

Objective

Build a system that imports permitted educational resources, indexes lecture transcripts, enables timestamp-based video search, and integrates with the existing online learning platform.

Three developers are working on the React frontend, Node.js backend, and Python AI/RAG service. This module must integrate with their implementations rather than duplicate existing functionality.

Technology Stack

- Python: Resource processing, transcript parsing, indexing utilities, and content health reporting.
- React: Existing frontend for displaying search results and video playback.
- Node.js and Express: Existing backend APIs and resource delivery.
- Python AI/RAG Service: Existing AI-powered retrieval and content search.
- Shared Database: Existing database for resource metadata and transcript records, as agreed with the backend developer.
- SRT/VTT: Timestamped transcript formats.

The implementation must follow the existing repository structure and conventions.

Features

1. Resource Importer

Build a utility that scans a configured directory and identifies supported educational resources:

- PDF documents
- Plain-text notes
- Video files
- Video transcripts

For each resource, collect:

- File name
- Resource type
- File size
- Relative storage path
- Processing status
- Course association, when available

Use a manifest or configuration file to map resources to courses. Do not rely exclusively on folder names for metadata.

Only import resources that the team has permission to store and redistribute.

2. Transcript Processing

Support timestamped transcripts in SRT and VTT formats.

The transcript processing pipeline must:

1. Parse timestamped transcript segments.
2. Associate transcripts with their corresponding videos and courses.
3. Normalize transcript text.
4. Preserve start and end timestamps.
5. Generate searchable records.
6. Report malformed or missing timestamps.

Videos may not always have transcripts. The system must handle missing transcripts gracefully.

Automatic speech recognition is out of scope unless the team explicitly agrees to implement it.

3. Timestamp-Based Video Search

Implement a search module that accepts a query and returns matching transcript segments.

Each search result should contain:

- Course ID
- Video/resource ID
- Video title
- Matching transcript text
- Start timestamp
- End timestamp
- Relevance score, if available

Use the existing shared database or an agreed interface to the backend.

Coordinate with the backend and AI developers before deciding where transcript indexes should be stored. Do not create a separate source of truth for courses or resources.

4. Video Playback Support

Verify that the existing backend can serve video files using HTTP range requests.

Test the following:

- Correct MIME types
- Seeking to a timestamp
- Playback from another device on the same network
- Multiple clients accessing the same video file

If video delivery is owned by the backend developer, provide reproducible tests and document required changes instead of implementing a duplicate delivery system.

Document how the frontend should construct timestamp-specific video links. Verify the chosen browser and playback implementation support the required seeking behavior.

5. Content Health Report

Implement a utility that generates both JSON output and a human-readable summary containing:

- Number of discovered resources
- Number of successfully processed resources
- Total storage usage
- Videos with transcripts
- Videos without transcripts
- Failed imports
- Indexing errors

All statistics must be calculated from actual files and processing results.

6. Application Integration

Coordinate with the backend developer on:

- Resource identifiers
- Course identifiers
- Storage paths
- Transcript metadata
- Search API contracts
- Video delivery URLs

Coordinate with the frontend developer on the response format required to display transcript search results and jump to video timestamps.

Coordinate with the AI developer so transcript data can be incorporated into the existing retrieval pipeline without creating conflicting indexes.

Do not change shared API contracts without agreement.

7. Online Verification

Create repeatable tests that verify:

1. Resources can be imported and processed.
2. Transcript indexing works correctly.
3. Transcript search returns relevant results.
4. Video files can be served through the existing backend.
5. Search results contain correct timestamps.
6. Video playback and seeking work through the frontend.
7. The content intelligence module integrates with the existing AI/RAG service.
8. The application works with an active internet connection and all required services available.

Offline verification is not part of the current implementation.

Proposed Project Structure

Adapt this structure to the existing repository. Reuse existing components wherever equivalent functionality already exists.

content-intelligence/
├── importer.py
├── transcript_parser.py
├── transcript_indexer.py
├── transcript_search.py
├── content_health.py
├── models.py
├── tests/
│   ├── test_parser.py
│   ├── test_search.py
│   ├── test_importer.py
│   └── test_content_health.py
├── sample-data/
│   ├── sample_transcript.srt
│   └── resource_manifest.json
├── docs/
│   └── integration.md
├── requirements.txt
└── README.md

Development Priority

Implementation should follow this order:

1. Parse one sample SRT/VTT transcript.
2. Index its segments.
3. Search for a phrase and return a timestamp.
4. Open the corresponding video at that timestamp.
5. Add resource importing.
6. Add content health reporting.
7. Integrate with the main application.

Start with the smallest working feature and expand incrementally.

Deliverables

- [ ] Resource import utility
- [ ] Transcript parser and indexer
- [ ] Timestamp-based search module
- [ ] Video delivery verification tests
- [ ] Content health report
- [ ] Sample transcript and metadata files suitable for public version control
- [ ] Integration documentation
- [ ] Automated tests
- [ ] README with setup and usage instructions

Development Guidelines

- Inspect the existing repository and database/API contracts before implementation.
- Use Python for processing utilities unless the team has established a different approach.
- Reuse the existing backend, database, and AI/RAG infrastructure.
- Do not introduce another database or duplicate existing functionality.
- Do not implement offline mode, offline caching, or offline verification.
- Do not commit large video files or other unnecessary binary resources to Git.
- Store environment-specific configuration in environment variables or appropriate configuration files.
- Do not commit secrets, API keys, credentials, or private educational resources.
- Use only educational content the team is authorized to store and redistribute.
- Run automated tests and document limitations honestly.
- Coordinate API and schema changes with the relevant developers.

Expected Outcome

A working online Content Intelligence module for NEXUS AI that imports educational resources, processes and indexes timestamped transcripts, supports searchable video timestamps, generates content health reports, and integrates with the existing React frontend, Node.js backend, and Python AI/RAG service.

The implementation should be modular, testable, maintainable, and compatible with the existing application architecture.
