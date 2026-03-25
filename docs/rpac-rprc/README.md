# RPAC/RPRC documentation

This is documentation of the [WE BUILD: WP4 QTSP group](../README.md).
Scope:

- Relying party access certificate issuance
- Relying party registration certificate issuance
This is documentation of the WE BUILD: WP4 QTSP group. Scope:
•	Relying party access certificate issuance
•	Relying party registration certificate issuance

Assumptions:
-	In complete eIDAS ecosystem, Registrars will act as Registration Authorities for TSP issuing RPAC & RPRC. Without any actual Registrar included in the project, present RAs of involved TSPs shall play the role of Registrar.
-	Registrars did not publish Registration policies yet. Considering WE BUILD as a close group where trust comes from belonging, registration policy shall rely on checking this belonging together with a public NCP registration policy for RPAC: TSP will check presence in lists of WP’s RP in place of national register of Wallet Relying Parties. WP’s leader shall establish these lists.

Issuance process:
Process of issuance will limit to AnnexD1 of TS 119 475 for MVP. It will be made of 11 steps :
1.	The user (RP representative) will connect and authenticate to TSP’s RA by using its EBW;
2.	RA will request credentials;
3.	And receive as an answer a EAA granting the user to act as representative of the RP (POA);
4.	RA contacts the user to request all additional attributes needed for producing RPRC;
5.	RA checks that the authenticated RP is present in the lists of authorised RP supplied by WP’s leaders;
6.	RA orders issuance of both certificates;
7.	CA issues RPAC and RPRC;
8.	CA transmits certificates to the RA;
9.	RA notifies the user (e.g. by e-mail);
10.	User authenticates via EBW;
11.	User retrieves RPAC & RPRC.

<img width="605" height="385" alt="image" src="https://github.com/user-attachments/assets/fc958e77-e107-4d42-84e3-cb6d5f5d8eb7" />

## Informative references

Standards:
-	EN 319 401
-	EN 319 411-1&2
-	TS 119 475
-	TS 119 411-8
-	IETF 7515
Technical Reports:
-	TR 119 411-9

