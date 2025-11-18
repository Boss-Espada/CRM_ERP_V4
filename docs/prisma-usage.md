# Prisma Database Setup and Usage Guide

This document explains how to use Prisma with the existing MySQL database in your project.

## Configuration

Prisma has been configured to use your existing MySQL database connection from `api/config.php`. The configuration is stored in the `.env` file:

```
DATABASE_URL="mysql://root:12345678@127.0.0.1:3306/mini_erp"
```

## Available Commands

The following npm scripts have been added to your `package.json` for easy database management:

- `npm run db:push` - Push schema changes to the database without migrations
- `npm run db:pull` - Pull schema from the database and update the Prisma schema file
- `npm run db:generate` - Generate the Prisma Client based on the schema
- `npm run db:studio` - Open Prisma Studio for browsing and editing your data
- `npm run db:seed` - Seed your database with sample data
- `npm run db:migrate` - Create and apply new migrations
- `npm run db:migrate:reset` - Reset the database and apply all migrations

## Prisma Schema

The Prisma schema is located at `prisma/schema.prisma`. It contains all your database models that were pulled from your existing MySQL database.

## Using Prisma in Your Application

To use Prisma in your application, import the client instance from `lib/prisma`:

```typescript
import prisma from '@/lib/prisma';

// Example: Get all users
const users = await prisma.user.findMany();

// Example: Create a new order
const newOrder = await prisma.order.create({
  data: {
    customer_id: 1,
    order_date: new Date(),
    status: 'pending',
    // ... other fields
  },
});
```

## Database Seeding

To seed your database with sample data, run:

```bash
npm run db:seed
```

The seed script is located at `scripts/seed-db.ts` and contains examples of how to create sample data for your database models.

## Working with Existing Database

Since you're working with an existing database, you'll primarily use the following workflow:

1. Make changes to your database directly (using a database tool, SQL queries, etc.)
2. Run `npm run db:pull` to update your Prisma schema with the latest database structure
3. Run `npm run db:generate` to regenerate the Prisma Client with the new schema

## Prisma Studio

Prisma Studio is a visual database browser that allows you to view and edit your data. To open it, run:

```bash
npm run db:studio
```

This will open a web interface at `http://localhost:5555` where you can browse your database tables, add, edit, and delete records.

## Troubleshooting

If you encounter issues with database connections:

1. Check that your MySQL server is running
2. Verify the database credentials in your `.env` file match those in `api/config.php`
3. Ensure the database name is correct

If you need to reset your database connection or reconfigure Prisma:

1. Update the `DATABASE_URL` in your `.env` file
2. Run `npm run db:pull` to test the connection and update the schema
3. Run `npm run db:generate` to regenerate the client

## Additional Resources

- [Prisma Documentation](https://www.prisma.io/docs/)
- [Prisma Client API Reference](https://www.prisma.io/docs/reference/api-reference/prisma-client-reference)
- [Prisma Schema Reference](https://www.prisma.io/docs/reference/api-reference/prisma-schema-reference)