Let's say we have an `enum` for a user's role. In my `schema.prisma` I'll define it as:
```
enum Role {
  AUTHOR
  EDITOR
  SUBSCRIBER
  ADMIN
}
```
But now I need to use it in my frontend and make a `select` that user can use to change his role.

So the first thing that I have to do is to `import` it as a value in my client:
![[enum-role-imported-as-value.jpg]]
As you can see, importing it like so will create a variable `Role` that I can loop over thanks to the `Object.values()` method:
```js
<select>
  {Object.values(Role).map((role: Role) => (
    <option
      className="cursor-pointer"
      value={role}
    >
      {role}
    </option>
  ))}
</select>
```
Since I am using T3 Stack and it implements the Zod library, I can create a schema that will validate the provided values:
```js
export const ProfileValuesSchema = z.object({
  // other fields
  role: z.nativeEnum(Role),
});

// We also create the type for the data of our form inferring those from the Zod definition
type ProfileValues = z.infer<typeof ProfileValuesSchema>;
```
The issue here, though, is that the form will send the selected value **as string**, while we want to keep the `type` strict. So when we send the data to our backend, we need to *massage* the data and tell TypeScript that the value will be part of the `enum` it expects.
```js
function onSubmit(data: ProfileValues) {
	data.role = Role[data.role] as Role;
	// Now you can send the data
}
```
To be brutally honest, I am not 100% sure that this is the "correct way", but it solved the issue I was facing while building my T3 app so, for now, it's good enough 😅

