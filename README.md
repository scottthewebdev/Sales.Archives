# Sales.Archives
Sales.Archives is the archival and retention module for the Sales backend. 

Approximate size

- Lines of code: ≈ 350 LOC (DbContext, entities, repository adapters, services, background workers, and helpers). This is an estimate and may change as the code evolves.

Key features

- Archive storage abstraction (IArchiveRepository) allowing multiple backends: database, file store, blob storage
- ArchiveDbContext: EF Core DbContext for archive tables and metadata
- Retention policy enforcement and purge worker to remove expired records
- Soft-delete tracking and optional hard-delete processes
- Export/import helpers for packaged archive bundles (JSON/zip) for offsite storage or restore
- Audit metadata with who/when/why for archived items
- Query and retrieval APIs for reading archived records

Services and components

- ArchiveService: core domain service for archiving, restoring, and querying archived entities
- IArchiveRepository: persistence abstraction with implementations for EF Core and file/Blob providers
- ArchiveDbContext: EF Core model and migrations for archive tables
- Models: ArchiveItem, ArchiveMetadata, ArchivePackage
- Background workers: Retention/PurgeWorker and ExportWorker
- DTOs and mapping helpers for import/export

Configuration

Typical configuration (appsettings.json):

- Archives:RetentionDays - default retention window before purge
- Archives:Provider - "EfCore" | "Blob" | "File"
- Archives:Blob:ConnectionString - provider specific settings
- Archives:ExportPath - filesystem path for exported packages

Running and building

Requirements: .NET 11 SDK

Build:
- dotnet build Sales.Archives

Database migrations (EF Core):
- dotnet ef migrations add Initial -p Sales.Archives -s <startup-project>
- dotnet ef database update -p Sales.Archives -s <startup-project>

Usage notes

- Register ArchiveService and chosen IArchiveRepository in the host DI container.
- Schedule Retention/PurgeWorker in the host application's background services (IHostedService).
- Prefer async methods for I/O-bound archive operations.

Extensibility

- Add new ArchiveRepository implementations (S3, Azure Blob, cold storage) via the IArchiveRepository contract
- Plug in encryption/compression for exported packages
- Integrate with long-term storage lifecycle systems for tiering

Contributing

- Add unit tests for retention enforcement and import/export behavior. Follow repository coding conventions.

License

- Refer to the repository license at the project root.
