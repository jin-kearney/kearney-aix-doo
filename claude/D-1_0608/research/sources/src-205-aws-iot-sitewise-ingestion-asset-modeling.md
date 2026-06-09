# src-205: Ingesting and Managing Data from Industrial Equipment with AWS IoT SiteWise

**URL:** https://aws.amazon.com/blogs/iot/ingesting-and-managing-data-from-industrial-equipment-with-aws-iot-sitewise/
**Title:** Ingesting and Managing Data from Industrial Equipment with AWS IoT SiteWise
**AccessDate:** 2026-06-09
**Published:** March 31, 2022 (AWS Official Blog)
**Related section:** Tool map (AWS IoT SiteWise), data ingestion, asset modeling

---

## Key Excerpts

**AWS IoT SiteWise Overview:**
- Managed service that simplifies collecting, organizing, and analyzing industrial equipment data at scale
- AWS IoT SiteWise Edge runs on-premises, securely connecting to and reading data from equipment or local historian databases

**Data Streams Concept:**
- New ingestion mode introduces a "Data stream" resource — time series data
- Equipment data represented as data streams when connected
- Disassociated data ingestion: all data streams ingested to cloud without prerequisite of associating to assets
- Customers can ingest through: AWS IoT SiteWise Edge gateway, AWS IoT Core, or directly using BatchPutAssetPropertyValue API

**Asset Modeling:**
- Asset model: virtual representation of a type of equipment with one or more measurement properties
- Asset hierarchy: organizes virtual representations of equipment across single facility or multiple facilities
- Bulk import/export/update via API endpoints: CreateMetadataTransferJob, ListMetadataTransferJobs, GetMetadataTransferJob
- "Virtuous cycle of asset modeling" — ingest first, model later, adapt as production environment evolves

**Model Mutability:**
- Data Stream can be moved between assets without copying data
- Supports frequent equipment moves (e.g., in discrete manufacturing, equipment physically moved to another location)
- Data retained during reconfiguration with no loss

**Access Control:**
- IAM policies with PropertyAliasPrefix condition — control which gateway can write to which data streams
- AssetHierarchyPath condition key — carve out roles for both administrators and operators
- Factory sites can be isolated from each other within same account using prefix-based policies

**Use Cases:**
- Monitor performance metrics from manufacturing lines, assembly robots, and factory equipment
- Assess equipment performance remotely across locations
- Identify gaps in production and waste
