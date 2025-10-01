## WE BUILD Draft: Collecting User Stories for application of QES/QEAA 

* Version: 0.1  
* Authors: Sander Dijkhuis (sander.dijkhuis@cleverbase.com), Iris van der Wel (iris.van.der.wel@cleverbase.com), Svilena Rakshieva (svilena.rakshieva@evrotrust.com)

# Abstract

This document proposes a structured approach by the QTSP group for collecting WE BUILD user stories and use case needs related to Qualified Electronic Signatures and Seals (QES) and (Qualified) Electronic Attestation of Attributes (QEAA).

# Terminology

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED",  "MAY", and "OPTIONAL" in this document are to be interpreted as described in [RFC 2119](#RFC2119).

# Background

The purpose of the WE BUILD project is to further the development of the EUDI wallet ecosystem by piloting a set of use cases. In order to support the stocktaking, specification and development of trust services in the WE BUILD use cases, and more specifically in terms of QES/QEAA at this stage, the QTSP group is collecting insights and feedback on what the use cases' needs will be.

The purpose of the collection of QES and QEAA user stories and needs, particularly in the earlier phases of WE BUILD, is to:

- Share knowledge with use cases regarding the possibilities of trust services.
- Gather input in a user-oriented manner.
- Prepare for the roadmap of the QTSP group.

This document proposes a structured approach for capturing both QEAA/QES user stories and needs. This SHALL be for exploratory purposes only and SHALL not commit contributors to implementing specific user stories or trust services.


# Collection of User Stories and needs for application of QES/QEAA

Input SHALL be collected in two  tables:

1. **User stories QES/QEAA**  
   This table captures user cases in the form of user stories.  
   Columns:  
   - **Responder's organization** – Identifies the organization providing the input, important for traceability and follow-up questions.  
   - **Use case #** – A unique identifier for each use case, which allows cross-referencing between tables and discussions.
   - **[will/ could/ will not] pilot** – Indicates whether the user story is planned to be piloted, helping alignment on scope, prioritize efforts and manage expectations.
   - **User story** (“As a [...] I want to [create/ validate/ enable] QES/QEAA of document type [...] so that [...]”) – Captures the functional requirement from the user perspective; central to understanding what the use case aims to achieve.
   - **Comments** – Provides additional context, clarifications, or assumptions, which can help the QTSP group interpret needs correctly.

   Reference sheet: [User stories QES/QEAA](https://docs.google.com/spreadsheets/u/1/d/1djcCeJJbmnMvfWQ2htW0bjEErrrELmfemJ4pG5Qh-aU)

2. **Use cases’ QEAA needs**  
   This table captures technical and organizational requirements to support QEAA.  
   Columns:  
   - **Responder's organization** – Identifies the organization providing the input, important for traceability and follow-up questions.  
   - **Use case #** – A unique identifier for each use case, which allows cross-referencing between tables and discussions.
   - **QEAA scheme name** – Identifies the specific scheme involved, important for technical mapping and validation. Align with Data schemes from [Attestation Rulebooks Catalog](https://github.com/eu-digital-identity-wallet/eudi-doc-attestation-rulebooks-catalog) where possible
   - **Authentic source** – Indicates the origin of the attribute.
   -  **QEAA provider** – Shows which party issues the electronic attestation can be a role or specific organization.
   -  **Wallet unit** – Indicates which wallet instance or environment is relevant, helping implementers coordinate deployment.
   -  **Relying party instance** – Identifies the specific instance consuming the QEAA.
   -  **Relying party** – Identifies the organization relying on the QEAA
   -  **Comments** – Provides additional context, clarifications, or assumptions, which can help the QTSP group interpret needs correctly.  

   Reference sheet: [Use cases’ QEAA needs](https://docs.google.com/spreadsheets/d/1rAqVdg3GGjuWVZqy0thjKPg6PG-qNGAeldjiPRhPens)

# Writing user stories and use case needs

  The sheets can be completed as follows:
  - The sheets MAY BE be completed by any WE BUILD member, and is definitely not restricted to the use case leads. We kindly invite the QTSP group members to provide their view on the use cases for knowledge sharing purposes.
  - The user stories SHALL be written from the perspective of the user, and not the technology provider. This allows us to focus on what the user wants to achieve, not on how the technology should implement it.  
  - Contributors SHALL use one row per entry, so data can be easily structured and analyzed afterwards.
  - Contributors SHALL provide their organization name to track contributions.
  
  As the use case needs are not yet crystallized, the sheets cannot be fully completed yet. Please try to think along with us about what the use cases might need in terms of trust services. Please be reminded that this as an exploratory mapping exercise and SHALL not commit contributors to implementing specific solutions in the use cases.

# References

- RFC 2119: https://datatracker.ietf.org/doc/html/rfc2119
- User stories QES/QEAA: https://docs.google.com/spreadsheets/u/1/d/1djcCeJJbmnMvfWQ2htW0bjEErrrELmfemJ4pG5Qh-aU
- Use cases’ QEAA needs: https://docs.google.com/spreadsheets/d/1rAqVdg3GGjuWVZqy0thjKPg6PG-qNGAeldjiPRhPens
  
