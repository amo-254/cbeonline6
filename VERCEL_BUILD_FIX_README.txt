CBE PMS - Vercel build adjustment

Changes made:
1. Removed the package.json postinstall command that ran `prisma generate` during dependency installation.
2. Moved Prisma Client generation into the build command:
   `prisma generate --schema=./prisma/schema.prisma && next build`
3. Removed prisma.config.ts so the Prisma CLI uses the explicit schema path in the scripts.
4. Pinned Node.js to 26.x to avoid automatic major-version upgrades.
5. Kept the Prisma schema and database models unchanged. No database reset or migration is run by this ZIP.

Deploy:
- Upload/replace the project files in the GitHub repository connected to Vercel.
- Ensure Vercel Root Directory points to the folder containing package.json (CBE_PMS_ONLINE_V1 if you upload the outer folder as-is).
- Ensure DATABASE_URL is set in Vercel Environment Variables for the target deployment environment.
- Redeploy.

Important:
This adjustment addresses the early Prisma postinstall stage shown in the supplied logs.
It does not prove that the entire application builds successfully; the full Vercel log after
dependency installation is still needed if another error appears.
Do NOT run `prisma db push`, reset, or seed against production unless you have separately
reviewed the migration and backed up the database.
