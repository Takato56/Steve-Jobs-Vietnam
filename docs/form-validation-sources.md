# Form Validation Sources

The pilot form controls deliberately distinguish legal facts from demo heuristics.

- Administrative-address controls now use a cascading province/city → district → ward/commune selector followed by a free-text detail field for house number, street/alley or village/group. The frontend reads these levels from `/api/v1/administrative-units/*`; the backend obtains them from the General Statistics Office administrative-unit service at `https://danhmuchanhchinh.nso.gov.vn/DMDVHC.asmx`.
- The 34-unit and 3,321-unit references below describe the post-reorganization legal catalog. Because the upstream SOAP service exposes province, district and ward methods, the current UI presents all three returned levels for compatibility. This technical hierarchy must not be represented as independent proof that every returned district remains a current legal administrative tier; source effective dates and upstream results must be checked before production submission.
- A personal identification number is checked only for presence and the public 12-digit format. GovEase does not infer or validate the first digits. In the target integration, identity data comes from the authenticated VNeID/National Public Service Portal session; manual entry remains only as a standalone-demo fallback.
- The requester relationship list follows the explicit family/caregiver groups in Article 15 of the 2026 Civil Status Law.
- Possible Telex residue such as `Nguyeenx` is a warning heuristic. It asks the citizen to compare the value with the original document; it is not represented as a legal rule.

Official references:

- Government administrative-unit catalog: https://xaydungchinhsach.chinhphu.vn/bang-danh-muc-va-ma-so-cua-34-tinh-thanh-moi-cac-don-vi-hanh-chinh-cap-xa-moi-11925070418263625.htm
- Official catalog of 3,321 commune-level units: https://xaydungchinhsach.chinhphu.vn/danh-sach-3321-don-vi-hanh-chinh-cap-xa-tai-34-tinh-thanh-sau-sap-xep-sap-nhap-119250710102358656.htm
- Civil Status Law 03/2026/QH16: https://xaydungchinhsach.chinhphu.vn/toan-van-luat-ho-tich-so-03-2026-qh16-119260527163142286.htm

## Effective-date warning

Law 03/2026/QH16 was passed on 23 April 2026 but takes effect on 1 March 2027. Until then, it is a future-law integration target, not the legal basis for claiming the current pilot procedure is already governed by its provisions. Its principles on one-time data provision and proactive database lookup are used only to shape the integration architecture.
