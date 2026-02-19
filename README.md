# Fichaje Gratis (Rapidgest)

Landing page built with Astro + Tailwind. It promotes the free time-tracking offer and captures leads into Google Sheets via Apps Script.

## Project Structure

```text
/
├── public/
│   └── Images/
├── src/
│   ├── components/
│   ├── pages/
│   └── styles/
├── astro.config.mjs
├── package.json
└── tsconfig.json
```

## Pages

- [src/pages/index.astro](src/pages/index.astro): Main landing page with all sections.
- [src/pages/aviso-legal.astro](src/pages/aviso-legal.astro): Legal notice.
- [src/pages/politica-privacidad.astro](src/pages/politica-privacidad.astro): Privacy policy.
- [src/pages/cookies.astro](src/pages/cookies.astro): Cookies policy.

## Components

- [src/components/SiteHeader.astro](src/components/SiteHeader.astro): Top navigation with CTA.
- [src/components/LeadForm.astro](src/components/LeadForm.astro): Lead form and Apps Script submission logic.
- [src/components/SocialProof.astro](src/components/SocialProof.astro): Logos/testimonials section.
- [src/components/StickyScroll.astro](src/components/StickyScroll.astro): Sticky scroll narrative block.
- [src/components/Features.astro](src/components/Features.astro): Feature grid.
- [src/components/InteractiveDemo.astro](src/components/InteractiveDemo.astro): Interactive mock demo.
- [src/components/RapidgestBanner.astro](src/components/RapidgestBanner.astro): ERP banner.
- [src/components/APISection.astro](src/components/APISection.astro): API features and CTA.
- [src/components/FAQ.astro](src/components/FAQ.astro): Frequently asked questions.
- [src/components/Footer.astro](src/components/Footer.astro): Footer links and copy.
- [src/components/BrandLockup.astro](src/components/BrandLockup.astro): Logo/brand lockup.

## Styles

- [src/styles/global.css](src/styles/global.css): Global styles and Tailwind setup.

## Environment Variables

Create a `.env` file in the project root:

```bash
PUBLIC_APPS_SCRIPT_URL=https://script.google.com/macros/s/XXXX/exec
```

The URL must be the Apps Script Web App URL ending in `/exec`.

## Apps Script Setup (Google Sheets)

1. Create a Google Sheet with columns: `nombre`, `empresa`, `email`, `telefono`, `fecha`, `interes`.
2. Create an Apps Script project and deploy as a Web App.
3. Use `openById` to target the sheet, then redeploy.

Example `doPost`:

```javascript
function doPost(e) {
	const SPREADSHEET_ID = "YOUR_SHEET_ID";
	const SHEET_NAME = "Hoja 1";
	const sheet = SpreadsheetApp.openById(SPREADSHEET_ID).getSheetByName(SHEET_NAME);

	const rowData = [
		e.parameter.nombre || "",
		e.parameter.empresa || "",
		e.parameter.email || "",
		e.parameter.telefono || "",
		new Date(),
		e.parameter.interes || "fichaje",
	];

	sheet.appendRow(rowData);

	return ContentService
		.createTextOutput(JSON.stringify({ result: "success" }))
		.setMimeType(ContentService.MimeType.JSON);
}
```

## Development

```bash
npm install
npm run dev
```

Local dev server runs at `http://localhost:4321/`.

## Production Build

```bash
npm run build
```

The static output is generated in `dist/`.

## Preview Build

```bash
npm run preview
```

## Deployment Notes

- Deploy the contents of `dist/` to any static host (Vercel, Netlify, Cloudflare Pages, etc.).
- Make sure `PUBLIC_APPS_SCRIPT_URL` is configured in the hosting environment.
