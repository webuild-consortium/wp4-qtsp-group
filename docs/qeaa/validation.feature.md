# Feature: EAA validation at RP

The TSP validates an EAA as a service to an RP.
The TSP does not necessarily return a determinate result.
In particular EAA that is not QEAA may be unsupported by EAA validation services.

## Scenario: Proxy model

The TSP validates an EAA on behalf of a RP, using the RP’s access certificate.

## Scenario: Async model

The RP sends static presented data to the TSP, which verifies the origin, authenticity, and validity of the EAA.
