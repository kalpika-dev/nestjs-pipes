# Installation

```bash
npm install --save @kalpika-dev/nestjs-pipes
```

# Usage

```typescript
// import pipes
import { WherePipe, OrderByPipe } from '@kalpika-dev/nestjs-pipes';
// import types
import { Pipes } from '@kalpika-dev/nestjs-pipes/index';
```

# #OrderByPipe

```typescript
@Query('orderBy', OrderByPipe) orderBy?: Pipes.Order,
```

### Examples:

* Sort the rows by the column `firstName` in ascending order
```
https://example.com/?sortBy=firstName:asc
```

Test case:
```typescript
  it('should convert value like "name:asc, address:desc" to { name: "asc", address: "desc" }', () => {
    const value = 'name:asc, address:desc';
    const result = pipe.transform(value);
    expect(result).toEqual({
      address: 'desc',
      name: 'asc',
    });
  });
```


# #WherePipe

## Operators realized:

**in** - `where=zipCode: in array(11111, 22222)`

**lt** - `where=age: lt int(12)`

**lt** - `where=age: lt int(12)`

**lte** - `where=age: lte int(12)`

**gt** - `where=age: gt int(12)`

**gte** - `where=age: gte int(12)`

**equals** - `where=age: equals int(12)`

**not** - `where=age: not int(12)`

**contains** - `where=firstName: contains string(John)` or `where=firstName: contains John`

**startsWith** - `where=firstName: startsWith string(John)` or `where=firstName: startsWith John`

**endsWith** - `where=firstName: endsWith string(John)` or `where=firstName: endsWith John`

**every** - `where=firstName: every string(John)` or `where=firstName: every John`

**some** - `where=firstName: some string(John)` or `where=firstName: some John`

**none** - `where=firstName: none string(John)` or `where=firstName: none John`

**OR** - `where=OR:[ firstName: contains Jhon, lastName: contains Doe]`

## Where types realized:

**string** - `where=firstName: contains string(John)`

**int** - `where=age: gt int(12)`

**float** - `where=age: gt float(12.5)`

**boolean** - `where=active: equals boolean(true)`

**bool** - `where=active: equals boolean(true)`

**date** - `where=createdAt: gt date(2019-01-01)`

**datetime** - `where=createdAt: gt datetime(2019-01-01 12:00:00)`

**array** - `where=zipCode: in array(int(111111), int(222222))`

```typescript
@Query('where', WherePipe) where?: Pipes.Where
```

### Examples:

* Select all the rows where the column `firstName` is equal to `John`
```
https://example.com/?where=firstName:John
```

* Select all the rows where createdAt is greater than 2023-01-13 12:04:27.689
```
https://example.com/?where=createdAt: gt date(2023-01-13 12:04:27.689)
```

* Select all rows where id is not equal to 1
```
https://example.com/?where=id: not int(12)
```

* Select all rows where id is greater than 1 and email contains `@gmail.com`
```
https://example.com/?where=id: gt int(1), email: contains @gmail.com
```

* Select all rows where ids are 1, 2 or 3 or age is 18
```
https://example.com/?where=OR:[id: in array(1,2,3), age: int(18)]
```

* Select all rows where the user's email contains `@gmail.com`.
```
https://example.com/?where=user.email: contains @gmail.com
```

* Select all users whose firstName contains `Admin` but exclude users whose roles are `CUSTOMER`.
```
https://example.com/?where=profile.firstName: contains Admin, NOT: roles: hasSome CUSTOMER
```

* Select user who has the email `super-admin@gmail.com` or `verified` users but exclude user whose email contains `test@gmail.com`.
```
https://example.com/?where=OR:[ email: contains super-admin@gmail.com, isVerified: equals boolean(true) ],  NOT: email: contains sendgridupboost@gmail.com
```

# WherePipe vs OrderByPipe

### Examples:

* Select all the rows where the column `firstName` is equal to `John` and sort the rows by the column `firstName` in ascending order
```
https://example.com/?where=firstName:John&sortBy=firstName:asc
```

Test case:
```typescript
  it('should parse "OR" with array values from string "OR:[id:in array(int(1),int(2),int(3)), user: contains Jhon]"', () => {
    const string =
      "OR:[id:in array(int(1),int(2),int(3)), user: contains Jhon]";

    expect(pipe.transform(string)).toEqual({
      OR: [
        {
          id: {
            in: [1, 2, 3]
          }
        },
        {
          user: {
            contains: "Jhon"
          }
        }
      ],
    });
  });
```
# #SelectPipe

```typescript
@Query('select', SelectPipe) select?: Pipes.Select
```

### Examples:

* Select the columns `firstName` and `lastName`
```
https://example.com/?select=firstName,lastName
```

* Exclude the columns `firstName` and `lastName`

```
https://example.com/?select=-firstName,-lastName
```
Test case:
```typescript
  it('should transform string user like { user:true }', () => {
    const value = 'user';

    const result = pipe.transform(value);

    expect(result).toEqual({
      user: true,
    });
  });
```
