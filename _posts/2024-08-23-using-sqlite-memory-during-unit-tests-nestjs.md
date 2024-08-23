---
layout: post
title:  "Using the sqlite in memory during unit tests with Nestjs"
date:   2024-08-23 15:49:00 -0200
categories: blog, tecnologia, javascript, react
---
Now I'll teach you how to use SQLite in memory for unit testing with NestJS. Utilizing SQLite in memory for unit tests brings many benefits: it's simple to use, well-known for proof of concepts, and compatible with many frameworks and tools. This approach is particularly useful for running tests as part of a continuous integration process due to its simplicity and lightweight nature. Additionally, it ensures an easy way to test across environments without loading more complex test configurations, preventing side effects and promoting repeatability, thereby improving the reliability and maintainability of tests. The advantage of this approach is that with minimal configuration, we can automate migrations and populate data in the same way as in other environments. This ensures that we can focus on writing and executing tests instead of creating substitutes to replace the data layer.
Utilizing SQLite in memory for unit tests offers many benefits: it's simple to use, widely recognized for proof of concepts, and compatible with many frameworks and tools. First, please ensure that Docker, SQLite, and Node.js are installed on your operating system. 
Here is the final structure of the application:

```
src/
├── infrastructure/
│   ├── database/
│   │   ├── database.module.ts
│   │   ├── entities/
│   │   ├── task.entity.ts
│   │   ├── in-memory/
│   │   ├── database.module.ts
│   │   ├── migrations/
│   │   ├── time-create-task-table.migration.ts
│   │   ├── mysql/
│   │   ├── mysql.module.ts
│   │   ├── repositories/
│   │   ├── task.repository.ts
│   │   ├── seeds/
│   │   ├── task-seed-service.ts
├── main/
|    ├── config/
│    │   ├── database/
│    │   |   ├── mysql.config.ts
│    │   |   ├── in-memory.config.ts
├── modules/
|   |── task/
│   │   ├── task.controller.spec.ts
│   │   ├── task.controller.ts
│   │   ├── task.service.spec.ts
│   │   ├── task.service.ts
├── app.module.ts
├── main.ts
├── .env
test/
├── jest-e2e.json
├── modules/
│   ├── task/
│   │   ├── task.e2e-spec.ts
```

Create the project using the Nest CLI: 

```
nest new nestj-with-sqlite-testing
```
If you don't have the Nest CLI installed, please run this command first:

```
npm i -g @nestjs/cli
```

Install the project dependencies using the commands below:

```
npm i --save @nestjs/typeorm typeorm mysql2 sqlite3 @nestjs/config
npm i -D @faker-js/faker
```

Next, we will use path aliases to make imports cleaner. Open the **tsconfig.json** file and add:

```json
{
  "compilerOptions": {
    "module": "commonjs",
    "declaration": true,
    "removeComments": true,
    "emitDecoratorMetadata": true,
    "experimentalDecorators": true,
    "allowSyntheticDefaultImports": true,
    "target": "ES2021",
    "sourceMap": true,
    "outDir": "./dist",
    "baseUrl": "./",
    "incremental": true,
    "skipLibCheck": true,
    "strictNullChecks": false,
    "noImplicitAny": false,
    "strictBindCallApply": false,
    "forceConsistentCasingInFileNames": false,
    "noFallthroughCasesInSwitch": false,
    "resolveJsonModule": true,
    "paths": {
      "@/*": ["src/*"],
      "@test/*": ["test/*"]
    }
  }
}
```

For this configuration to work, it will be necessary to configure **moduleNameMapper** in Jest. Open the package.json, cut the **Jest configurations**, and create a **jest.config.ts** file at the same level as **package.json** to configure unit tests. The Jest scope should look like this:

```typescript
module.exports = {
  preset: 'ts-jest', 
  moduleFileExtensions: ['js', 'json', 'ts', 'node'],
  rootDir: 'src',
  testRegex: '.*\\.spec\\.ts$',
  transform: {
    '^.+\\.(t|j)s$': 'ts-jest',
  },
  moduleNameMapper: {
    '^@/(.*)$': '<rootDir>/$1',
    '^@test/(.*)$': '<rootDir>/test/$1',
  },
  collectCoverageFrom: ['**/*.(t|j)s'],
  coverageDirectory: '../coverage/',
  testEnvironment: 'node',
  coveragePathIgnorePatterns: ['main.ts', '.module.ts$'],
};
```
Add the configurations on **test/jest-e2e.json**: 

```json
{
  "moduleFileExtensions": ["js", "json", "ts"],
  "rootDir": "../",
  "testEnvironment": "node",
  "testRegex": ".e2e-spec.ts$",
  "transform": {
    "^.+\\.(t|j)s$": "ts-jest"
  },
  "moduleNameMapper": {
    "^@/(.*)$": "<rootDir>/src/$1",
    "^@test/(.*)$": "<rootDir>/test/$1"
  },
  "collectCoverage": true,
  "coverageDirectory": "<rootDir>/coverage/e2e",
  "collectCoverageFrom": [
    "src/**/*.(t|j)s"
  ],
  "coveragePathIgnorePatterns": [
    "/node_modules/",
    "/dist",
    "/test/",
    "coverage",
    ".spec.ts"
  ],
  "coverageReporters": [
    "json",
    "lcov",
    "text",
    "clover"
  ]
}
```

Create the **docker-compose.yaml** file and the **data** folder for Docker to store the container's data at the same level as **package.json**:

```dockerfile
version: '3.8'
services:
  mysql:
    image: mysql:8.0
    container_name: mysql
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: nestjs_testing
      MYSQL_USER: user
      MYSQL_PASSWORD: user
    volumes:
      - ./data:/var/lib/mysql
    ports:
      - "9000:3306"
```

Bring up the MySQL container using the following command: 

```
docker-compose up -d
```

If you want to check if the container is healthy, use the command **docker ps** to see the list of running containers. Later, we will create two modules: one for the application and another for the database infrastructure, with these commands:

```
nest g module infrastructure/database
nest g module infrastructure/database/mysql
nest g module infrastructure/database/in-memory
nest g module modules 
nest g controller modules/task
nest g service modules/task
```

**Warning**: Instead of creating the "task" folder inside the "modules" folder, you can create a separate module named "task" to better organize and separate the modules. The difference will be that 'task' will have its own file named **task.module.ts**.
After that, you need to create the configuration path **src/main/config/database** and add the configuration files. First, create the **in-memory-database.config.ts** file:

```typescript
import { registerAs } from '@nestjs/config';
import { TypeOrmModuleOptions } from '@nestjs/typeorm';

export const inMemoryDatabaseConfig = registerAs(
  'database.inMemory',
  (): TypeOrmModuleOptions => ({
    type: 'sqlite',
    database: ':memory',
    entities: [
      __dirname +
        '/../../../infrastructure/database/entities/*.entity{.ts,.js}',
    ],
    migrations: [
      __dirname +
        '/../../../infrastructure/database/migrations/*.migration{.ts,.js}',
    ],
    synchronize: true,
    logging: ['error', 'query', 'warn', 'info', 'schema', 'log'],
  }),
);
```

Now, create the **mysql-database.config.ts** file at the same path as **in-memory-database.config.ts**:

```typescript
import { registerAs } from '@nestjs/config';
import { TypeOrmModuleOptions } from '@nestjs/typeorm';

export const mysqlDatabaseConfig = registerAs(
  'database.mysql',
  (): TypeOrmModuleOptions => ({
    type: 'mysql',
    host: process.env.DB_HOST,
    username: process.env.DB_USERNAME,
    password: process.env.DB_PASSWORD,
    database: process.env.DB_DATABASE,
    port: Number(process.env.DB_PORT),
    entities: [
      __dirname +
        '/../../../infrastructure/database/entities/*.entity{.ts,.js}',
    ],
    migrations: [
      __dirname +
        '/../../../infrastructure/database/migrations/*.migration{.ts,.js}',
    ],
    synchronize: true,
    logging: ['error', 'query', 'warn', 'info', 'schema', 'log'],
  }),
);
```

So, we can transform the database module into a dynamic module. The logic is simple: if **NODE_ENV** is equal to "**test**", the database module will import and export the InMemoryDatabase module. Otherwise, it will import and export the MysqlDatabase module. Here is the code for **src/infrastructure/database.module.ts**:

```typescript
import { DynamicModule, Module } from '@nestjs/common';
import { MysqlModule } from './mysql/mysql.module';
import { InMemoryModule } from './in-memory/in-memory.module';

@Module({
  imports: [MysqlModule, InMemoryModule],
})
export class DatabaseModule {
  static forRoot(): DynamicModule {
    const isTestEnv = process.env.NODE_ENV === 'test';
    const moduleImports = isTestEnv ? [InMemoryModule] : [MysqlModule];
    console.log(moduleImports, isTestEnv);
    return {
      module: DatabaseModule,
      imports: moduleImports,
      exports: moduleImports,
    };
  }
}
```

Before continuing with the database tools such as migrations, seeds, and others, create the entity to map its properties. The file should be **src/infrastructure/entities/task.entity.ts**:

```typescript
import { Entity, Column, PrimaryGeneratedColumn } from 'typeorm';

@Entity()
export class Task {
  @PrimaryGeneratedColumn()
  id: number;

  @Column()
  name: string;

  @Column()
  done: number;
}
```

Now, to complete the database implementation, you need to create the migration, repository, and seed to insert values into the table. First, create the migration using the following command:

```
npx typeorm migration:create src/infrastructure/database/migrations/create-task-table.migration
```

After creating the migration file, you need to define the types and columns in the up method of the migration class and drop (or destroy) the table in the down method. **src/infrastructure/database/migrations/create-task-table.migration.ts**:

```typescript
import { MigrationInterface, QueryRunner, Table } from 'typeorm';

export class CreateTaskTable1722825209848 implements MigrationInterface {
  public async up(queryRunner: QueryRunner): Promise<void> {
    await queryRunner.createTable(
      new Table({
        name: 'task',
        columns: [
          {
            name: 'id',
            type: 'int',
          },
          {
            name: 'name',
            type: 'varchar',
          },
          {
            name: 'done',
            type: 'int',
            default: 0,
          },
        ],
      }),
    );
  }

  public async down(queryRunner: QueryRunner): Promise<void> {
    await queryRunner.dropTable('task');
  }
}
```

Now, create the seed to populate the table with fake data using the **faker-js** library. To do this, create a **seeds** folder inside **src/infrastructure/database**, and within that folder, create the **task-seed-service.ts** file. The file path is **src/infrastructure/database/seeds/task-seed-service.ts**:

```typescript
import { Injectable } from '@nestjs/common';
import { EntityManager } from 'typeorm';
import { faker } from '@faker-js/faker';
import { Task } from '@/infrastructure/database/entities/task.entity';

@Injectable()
export class TaskSeedService {
  private done = [0, 1];

  constructor(private readonly entityManager: EntityManager) {}

  async seed() {
    const tasks = Array.from({ length: 10 }).map(() => {
      const randomIndex = Math.floor(Math.random() * this.done.length);
      const task = new Task();
      task.name = faker.word.verb();
      task.done = this.done[randomIndex];
      return task;
    });
    await this.entityManager.save(tasks);
  }
}
```

After that, it's time to implement the repository. Create a repositories folder inside **src/infrastructure/database**, and within that folder, create the **task.repository.ts** file. The file path is **src/infrastructure/database/repositories/task.repository.ts**:

```typescript
import { Injectable } from '@nestjs/common';
import { InjectRepository } from '@nestjs/typeorm';
import { Repository } from 'typeorm';
import { Task } from '../entities/task.entity';

@Injectable()
export class TaskRepository {
  constructor(
    @InjectRepository(Task)
    private repository: Repository<Task>,
  ) {}

  async findByStatus(status: string): Promise<Task[]> {
    return await this.repository
      .createQueryBuilder('task')
      .select()
      .where('task.status = :status', { status })
      .getMany();
  }
}
```

Now we need to import the repository, seed, and **TypeOrmModule** into both the **InMemoryDatabase** module and the **MysqlDatabase** module.
For the **InMemoryDatabase** module, see the configuration here. **src/infrastructure/database/in-memory/in-memory.module.ts**:

```typescript
import { Module } from '@nestjs/common';
import { TypeOrmModule, TypeOrmModuleOptions } from '@nestjs/typeorm';
import { ConfigModule, ConfigService } from '@nestjs/config';
import { inMemoryDatabaseConfig } from '@/main/config/database/in-memory-database.config';
import { TaskRepository } from '@/infrastructure/database/repositories/task.repository';
import { TaskSeedService } from '@/infrastructure/database/seeds/task-seed-service';
import { Task } from '@/infrastructure/database/entities/task.entity';

@Module({
  imports: [
    ConfigModule.forRoot({
      isGlobal: true,
      load: [inMemoryDatabaseConfig],
    }),
    TypeOrmModule.forRootAsync({
      inject: [ConfigService],
      useFactory: (configService: ConfigService): TypeOrmModuleOptions =>
        configService.get<TypeOrmModuleOptions>('database.inMemory'),
    }),
    TypeOrmModule.forFeature([Task]),
  ],
  providers: [TaskRepository, TaskSeedService],
  exports: [TaskRepository, TaskSeedService],
})
export class InMemoryModule {}
```

For the **MysqlDatabase** module, see the configuration here:

```typescript
import { Module } from '@nestjs/common';
import { TypeOrmModule, TypeOrmModuleOptions } from '@nestjs/typeorm';
import { ConfigModule, ConfigService } from '@nestjs/config';
import { mysqlDatabaseConfig } from '@/main/config/database/mysql-database.config';
import { TaskRepository } from '@/infrastructure/database/repositories/task.repository';
import { TaskSeedService } from '@/infrastructure/database/seeds/task-seed-service';
import { Task } from '@/infrastructure/database/entities/task.entity';

@Module({
  imports: [
    ConfigModule.forRoot({
      isGlobal: true,
      load: [mysqlDatabaseConfig],
    }),
    TypeOrmModule.forRootAsync({
      inject: [ConfigService],
      useFactory: (configService: ConfigService): TypeOrmModuleOptions =>
        configService.get<TypeOrmModuleOptions>('database.mysql'),
    }),
    TypeOrmModule.forFeature([Task]),
  ],
  providers: [TaskRepository, TaskSeedService],
  exports: [TaskSeedService],
})
export class MysqlModule {}
```

Now, we are going to configure the first test file, even though it doesn't have any endpoints. Our configuration will create the database using SQLite in memory, run the migration and seed, and destroy them after the tests are finished. The file path is **src/modules/task/task.service.spec.ts**:

```typescript
import { Test, TestingModule } from '@nestjs/testing';
import { TypeOrmModule } from '@nestjs/typeorm';
import { Connection } from 'typeorm';
import { TaskSeedService } from '../../infrastructure/database/seeds/task-seed-service';
import { TaskService } from './task.service';
import { Task } from '../../infrastructure/database/entities/task.entity';
import { DatabaseModule } from '../../infrastructure/database/database.module';

describe('TaskService', () => {
  let service: TaskService;
  let taskSeedService: TaskSeedService;
  let connection: Connection;

  beforeAll(async () => {
    const module: TestingModule = await Test.createTestingModule({
      imports: [DatabaseModule.forRoot(), TypeOrmModule.forFeature([Task])],
      providers: [TaskService],
    }).compile();

    service = module.get<TaskService>(TaskService);
    taskSeedService = module.get<TaskSeedService>(TaskSeedService);

    connection = module.get<Connection>(Connection);
    await connection.runMigrations();
    await taskSeedService.seed();
  }, 10000);

  afterAll(async () => {
    console.log('my connection', connection['@instanceof']);
    if (connection.isConnected) {
      let executedMigrations = await connection.query(
        'SELECT * FROM migrations ORDER BY timestamp DESC',
      );
      while (executedMigrations.length > 0) {
        await connection.undoLastMigration();
        executedMigrations = await connection.query(
          'SELECT * FROM migrations ORDER BY timestamp DESC',
        );
      }
      await connection.close();
    }
  }, 10000);

  it('should be defined', () => {
    expect(service).toBeDefined();
  });
});
```

You can now run the test using the following command:

```
npm run test 
```

Or to run a specific test file:

```
npm run test src/modules/task/task.service.spec.ts
```

Although there are some steps to configure, we can now see some results from the previous steps. Next, we will create our endpoint after configuring the controller's test file. The endpoint will use the HTTP GET verb and will accept a query parameter of type number to filter and return an array of tasks based on their completion status (0 for false and 1 for true).
The following files contain the implementation. 

**src/modules/task/task.controller.ts**:

```typescript
import { Controller, Get, Query, Req, Res, HttpStatus } from '@nestjs/common';
import type { Request, Response } from 'express';
import { TaskService } from '@/modules/task/task.service';

@Controller('tasks')
export class TaskController {
  constructor(private readonly taskService: TaskService) {}

  @Get()
  async searchTaskByState(
    @Query('done') done: string,
    @Req() _req: Request,
    @Res() res: Response,
  ): Promise<void> {
    const doneParam = done !== undefined ? Number(done) : 1;
    const data = await this.taskService.execute(doneParam);
    if (data) {
      res.status(HttpStatus.OK).json(data);
    } else {
      res.status(HttpStatus.NOT_FOUND).json({
        message: 'No tasks found with this state',
      });
    }
  }
}
```

**src/modules/task/task.service.ts**:

```typescript
import { Injectable } from '@nestjs/common';
import { TaskRepository } from '@/infrastructure/database/repositories/task.repository';

@Injectable()
export class TaskService {
  constructor(private readonly taskRepository: TaskRepository) {}

  async execute(done: number) {
    const data = await this.taskRepository.findByState(done);
    if (data.length) {
      return data;
    }
    return null;
  }
}
```

**src/modules/modules.module.ts: **:

```typescript
import { Module } from '@nestjs/common';
import { DatabaseModule } from '@/infrastructure/database/database.module';
import { TaskController } from '@/modules/task/task.controller';
import { TaskService } from '@/modules/task/task.service';

@Module({
  imports: [DatabaseModule.forRoot()],
  controllers: [TaskController],
  providers: [TaskService],
})
export class ModulesModule {}
```

Now, it’s time to configure the controller's test file. Here is the configuration. **src/modules/task/task.controller.spec.ts**:

```typescript
import { Test, TestingModule } from '@nestjs/testing';
import { TaskController } from './task.controller';
import { TaskService } from './task.service';
import { ModulesModule } from '../modules.module';
import { DatabaseModule } from '@/infrastructure/database/database.module';

describe('TaskController', () => {
  let controller: TaskController;

  beforeEach(async () => {
    const module: TestingModule = await Test.createTestingModule({
      imports: [ModulesModule, DatabaseModule.forRoot()],
      controllers: [TaskController],
      providers: [TaskService],
    }).compile();

    controller = module.get<TaskController>(TaskController);
  });

  it('should be defined', () => {
    expect(controller).toBeDefined();
  });
});
```

Before continuing to add more test cases, let's configure our end-to-end tests. Create a folder following the same structure as the application inside the src/test folder. The resulting structure will be: **src/test/module/task**. Inside this folder, create the **task.e2e-spec.ts** file.
We will now create two end-to-end test cases: one that filters tasks that are done and another that filters tasks that are not done. **src/test/module/task/task.e2e-spec.ts**:

```typescript
import { Test, TestingModule } from '@nestjs/testing';
import { HttpStatus, INestApplication } from '@nestjs/common';
import * as request from 'supertest';
import { Connection } from 'typeorm';
import { ModulesModule } from '@/modules/modules.module';
import { TaskSeedService } from '@/infrastructure/database/seeds/task-seed-service';

describe('TaskController (e2e)', () => {
  let app: INestApplication;
  let taskSeedService: TaskSeedService;
  let connection: Connection;

  beforeAll(async () => {
    const moduleFixture: TestingModule = await Test.createTestingModule({
      imports: [ModulesModule],
    }).compile();

    app = moduleFixture.createNestApplication();
    taskSeedService = app.get<TaskSeedService>(TaskSeedService);

    connection = app.get<Connection>(Connection);
    await connection.runMigrations();
    await taskSeedService.seed();
    await app.init();
  });

  afterAll(async () => {
    if (connection.isConnected) {
      let executedMigrations = await connection.query(
        'SELECT * FROM migrations ORDER BY timestamp DESC',
      );
      while (executedMigrations.length > 0) {
        await connection.undoLastMigration();
        executedMigrations = await connection.query(
          'SELECT * FROM migrations ORDER BY timestamp DESC',
        );
      }
      await connection.close();
    }
  });

  it('should return 200 when / (GET) is called with done equal to 1', () => {
    const done = '1';
    return request(app.getHttpServer())
      .get(`/tasks?done=${done}`)
      .expect(HttpStatus.OK);
  });

  it('should return 404 when / (GET) is called with done equal to 5', () => {
    const done = '5';
    return request(app.getHttpServer())
      .get(`/tasks?done=${done}`)
      .expect(HttpStatus.NOT_FOUND);
  });

  it('should return 200 when / (GET) is called without the query parameter done', () => {
    return request(app.getHttpServer()).get(`/tasks`).expect(HttpStatus.OK);
  });
});
```

Now we will complete the service test cases. **src/modules/task/task.service.spec.ts**:

```typescript
import { Test, TestingModule } from '@nestjs/testing';
import { TypeOrmModule } from '@nestjs/typeorm';
import { Connection } from 'typeorm';
import { TaskSeedService } from '../../infrastructure/database/seeds/task-seed-service';
import { TaskService } from './task.service';
import { Task } from '../../infrastructure/database/entities/task.entity';
import { DatabaseModule } from '@/infrastructure/database/database.module';

describe('TaskService', () => {
  let service: TaskService;
  let taskSeedService: TaskSeedService;
  let connection: Connection;

  beforeAll(async () => {
    const module: TestingModule = await Test.createTestingModule({
      imports: [DatabaseModule.forRoot(), TypeOrmModule.forFeature([Task])],
      providers: [TaskService],
    }).compile();

    service = module.get<TaskService>(TaskService);
    taskSeedService = module.get<TaskSeedService>(TaskSeedService);

    connection = module.get<Connection>(Connection);
    await connection.runMigrations();
    await taskSeedService.seed();
  });

  afterAll(async () => {
    if (connection.isConnected) {
      let executedMigrations = await connection.query(
        'SELECT * FROM migrations ORDER BY timestamp DESC',
      );
      while (executedMigrations.length > 0) {
        await connection.undoLastMigration();
        executedMigrations = await connection.query(
          'SELECT * FROM migrations ORDER BY timestamp DESC',
        );
      }
      await connection.close();
    }
  });

  it('should be defined', () => {
    expect(service).toBeDefined();
  });

  it('should return result when done equal to 1', async () => {
    const data = await service.execute(1);
    expect(data).toBeInstanceOf(Array);
  });

  it('should return result when done equal to 0', async () => {
    const data = await service.execute(0);
    expect(data).toBeInstanceOf(Array);
  });

  it('should return result when done equal to 5', async () => {
    const data = await service.execute(5);
    expect(data).toBeNull();
  });
});
```

Particularly, this approach can be useful for running tests as part of a continuous integration process due to its simplicity and lightweight nature. Although there are other options, such as **test containers**, this solution works well and can be used if your continuous integration environment doesn't support test containers. One downside is that SQLite has limited data types, which might necessitate creating two separate repository files: one for the app's database and another for SQLite. However, in my opinion, this approach can help with dependency inversion and can be a good first step toward improving the quality of the project.

Furthermore, it provides a straightforward way to test across environments without requiring complex test configurations. This approach helps prevent side effects and promotes repeatability, thereby improving the reliability and maintainability of tests. Even if you need to use multiple database flavors depending on your application, you will establish a pattern for testing features, whether for unit or end-to-end tests.

The advantage of this approach is that with minimal configuration, we can automate migrations and populate data. This allows us to focus on writing and executing tests rather than creating test doubles to replace the data layer. Sometimes, mocks can bypass parts of the code and increase testing complexity. If you have a coverage goal, in my opinion, this approach or using test containers can lead to more effective tests. Here is the <a href="https://github.com/paulodutra/nestjs-with-sqlite-testing" target="__blank">GitHub link</a>

Whenever we need to modify the application, we should ensure that the changes do not introduce problems or side effects. We can use tests with well-defined scenarios to make things safer. To simulate these scenarios, having access to the appropriate data can be very useful.
