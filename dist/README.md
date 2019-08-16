## Description

The [Couchbase](https://www.couchbase.com/) module for Nest framework

## Installation

```bash
$ npm i @scalio-oss/nest-couchbase couchbase
```

## Usage

`@scalio-oss/nest-couchbase` uses [couchbase](https://www.npmjs.com/package/couchbase) as a data provider and the `Repository` pattern to handle all items (documents) related operations.

First, let's create an `Entity`:

```typescript
import { Entity } from '@scalio-oss/nest-couchbase';

@Entity('cats')
export class Cat {
  name: string;
}
```

Where `cats` is the Couchbase bucket name.

Then, we need to import `CouchbaseModule` in our root `AppModule`:

```typescript
import { Module } from '@nestjs/common';
import { CouchbaseModule } from '@scalio-oss/nest-couchbase';

@Module({
  imports: [
    CouchbaseModule.forRoot({
      url: 'couchbase://127.0.0.1',
      username: 'couchbase',
      password: 'couchbase',
      bucket: 'test',
    }),
  ],
})
export class AppModule {}
```

In our `CatsModule` we need to initiate repository for our `Cat` entity:

```typescript
import { Module } from '@nestjs/common';
import { CouchbaseModule } from '@scalio-oss/nest-couchbase';
import { CatsService } from './cats.service';
import { CatsController } from './cats.controller';
import { Cat } from './cat.entity';

@Module({
  imports: [CouchbaseModule.forFeature([Cat])],
  providers: [CatsService],
  controllers: [CatsController],
})
export class CatsModule {}
```

And here is the usage of the repository in the service:

```typescript
import { Injectable } from '@nestjs/common';
import { InjectRepository, Repository } from '@scalio-oss/nest-couchbase';
import { Cat } from './cat.entity';

@Injectable()
export class CatsService {
  constructor(
    @InjectRepository(Cat)
    private readonly catsRepository: Repository<Cat>,
  ) {}

  findOne(id: string): Promise<Cat> {
    return this.catsRepository.get(id);
  }
}
```

## License

[MIT](LICENSE)

## Credits

Created by [@zMotivat0r](https://github.com/zMotivat0r) @ [Scalio](https://scal.io/)

<p align="center">
    <br/>
    <a href="https://scal.io/">
        <img src="https://raw.githubusercontent.com/scalio/bazel-nestjs-starter/master/readme-assets/scalio.png"/>
    </a>
</p>