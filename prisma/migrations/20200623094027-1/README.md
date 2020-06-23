# Migration `20200623094027-1`

This migration has been generated at 6/23/2020, 9:40:27 AM.
You can check out the [state of the schema](./schema.prisma) after the migration.

## Database Steps

```sql
CREATE TABLE "test"."World" (
"id" SERIAL,"name" text  NOT NULL ,"population" Decimal(65,30)  NOT NULL ,
    PRIMARY KEY ("id"))

CREATE UNIQUE INDEX "World.name" ON "test"."World"("name")
```

## Changes

```diff
diff --git schema.prisma schema.prisma
migration ..20200623094027-1
--- datamodel.dml
+++ datamodel.dml
@@ -1,0 +1,14 @@
+datasource db {
+  provider = "postgresql"
+  url = "***"
+}
+
+generator prisma_client {
+  provider = "prisma-client-js"
+}
+     
+model World {
+  id         Int    @id @default(autoincrement())
+  name       String @unique
+  population Float
+}
```


