# Typescript Utility Types Cheat Sheet

## Object Types

```ts

type User = {
  id: number;
  name: string;
  email: string;
  age: number;
};

type PartialUser = Partial<User>; // { id?: number; name?: string; email?: string; age?: number; }
type RequiredUser = Required<User>; // { id: number; name: string; email: string; age: number; }
type ReadonlyUser = Readonly<User>; // { readonly id: number; readonly name: string; readonly email: string; readonly age: number; }

type OmitUser = Omit<User, 'id' | 'name'>; // { email: string; age: number; }
type PickUser = Pick<User, 'id' | 'name'>; // { id: number; name: string; }

type PartiallyPickUser = Partial<
  Pick<User, 'id' | 'name'>
>; // { id?: number; name?: string; }

// 👇 "Mutable<T>" is a utility type that removes the "readonly" modifier from all properties of a given type "T".
type Mutable<T> = {
  -readonly [P in keyof T]: T[P];
}; // { id: number; name: string; email: string; age: number; }

```

## Union Types

```ts

type Role = 'admin' | 'user' | 'anonymous';

type NonAdminRole = Exclude<Role, 'admin'>; // 'user' | 'anonymous'

type RoleAttribute = 
  | { role: 'admin'; orgId: string; }
  | { role: 'user' }
  | { role: 'anonymous' };

type AdminRole = Extract<
  RoleAttribute,
  { role: 'admin' }
>; // { role: 'admin'; orgId: string; }

```

## Function Types

```ts

type Fn = (a: number, b: string) => number;

type ReturnType = ReturnType<Fn>; // 👈 Returns the return type of a function type. number
type Params = Parameters<Fn>; // 👈 Returns the parameters of a function type as a tuple. [number, string]
/* 👆 
  "ReturnType" and "Parameters" is useful when you have a function in an external library and you need to extract out the types of its parameters or return type which if the library doesn't provide you with the types.
*/

type MaybeString = string | null | undefined;
type DefinitelyString = NonNullable<MaybeString>; // 👈 Removes null and undefined from a type. string

type PromiseString = Promise<string>;
type Result = Awaited<PromiseString>; // 👈 Unwraps a Promise type. string
```

## Example

```ts
const func = async () => {
  return {
    id: 123,
  };
}

type Result = Awaited<ReturnType<typeof func>>; // 👈 { id: number; }
```
