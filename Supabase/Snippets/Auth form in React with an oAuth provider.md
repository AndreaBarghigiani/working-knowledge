## Full code
```js
import * as React from 'react';
import { Button } from '@chakra-ui/react';

import { useUser } from '../lib/UserContext';
import { supabase } from '../lib/Store';

const Auth = () => {
	const [user, setUser] = useUser();
	const haveUser = !user;

	// Check if we have a user logged in
	React.useEffect(() => {
		const { data: authListener } = supabase.auth.onAuthStateChange(
			async (event, session) => {
				setUser(session?.user ?? null);
			}
		);

		// Cleanup function
		return () => {
			authListener.unsubscribe();
		};
	}, [setUser]);

	const handleClick = async e => {
		e.preventDefault();
		switch (e.target.name) {
			case 'login':
				await supabase.auth.signIn({ provider: 'bitbucket' });
				break;
			case 'logout':
				await supabase.auth.signOut();
				break;
			default:
				break;
		}
	};

	const colorButton = haveUser ? 'red' : 'green';
	const textButton = haveUser ? 'Logout' : 'Login';
	const nameButton = haveUser ? 'logout' : 'login';

	return (
		<>
			<Button
				name={nameButton}
				type="submit"
				onClick={handleClick}
				colorScheme={colorButton}
			>
				{textButton}
			</Button>
		</>
	);
};

export default Auth;
```

## Explanation
Authentication in Supabase is pretty simple, basically everything is already in place and you have some functions that will help you handle everything:
* `auth.signUp` - creates a new user and as default will send to the user a confirmation email
* `auth.signIn` - log in an existing user (in case of oAuth will behave as `auth.signUp`)
* `auth.signOut` - log out the user from the platform

For the login/registration phase you can change the redirect on success that defaults to `localhost:3000` inside your Authentication -> Settings in [app.supabase.io](app.supabase.io).

If you aim to offer a login system with one of the supported oAuth providers

The following snippet of code is built with the [Chakra UI](https://chakra-ui.com/) but you can build easily with simple HTML or any other solution, the UI is not the important stuff here.

```js
const Auth = () => {
	const [user, setUser] = useState(null);
	const haveUser = !user;
	
	const handleClick = async e => {
		e.preventDefault();
		switch (e.target.name) {
			case 'login':
				await supabase.auth.signIn({ provider: 'bitbucket' });
				break;
			case 'logout':
				await supabase.auth.signOut();
				break;
			default:
				break;
		}
	};

	const colorButton = haveUser ? 'red' : 'green';
	const textButton = haveUser ? 'Logout' : 'Login';
	const nameButton = haveUser ? 'logout' : 'login';

	return (
		<>
			<Button
				name={nameButton}
				type="submit"
				onClick={handleClick}
				colorScheme={colorButton}
			>
				{textButton}
			</Button>
		</>
	);
};
```

As you can see code is really simple, I use a bit of logic to style the button and change it's behavior once clicked but the interesting part is that, since I use Bitbucket as oAuth provider **I do not have to send the email and password** via the fns used.

So instead of the object `{ email, password }` all I have to do is to set its `provider` property and specify that I want to use Bitbucket as provider with `{ provider: 'bitbucket' }`.

In this code there is a problem though...

The thing that I was missing was that you **need to listen for changes in the login status of the user** with `auth.onAuthStateChange` because if not it get hard for your application to know when the user is signed in. During my tests I did solve it with a [[setTimeout]] but you can't rely on that 😂

So since I am using React all I have to do was to fire an [[React.useEffect()|useEffect]] and update the state of the component when `auth.onAuthStateChange` was fired.

```js
// Check if we have a user logged in
React.useEffect(() => {
	const { data: authListener } = supabase.auth.onAuthStateChange(
		async (event, session) => {
			setUser(session?.user ?? null);
		}
	);

	// Cleanup function
	return () => {
		authListener.unsubscribe();
	};
}, [setUser]);
```

This is the piece of magic that let's your app know if the user is logged in or not. 

I am destructuring `authListener` so I'll be able to run its `unsubscribe` fn when the component will unmount and update the state if we have the information of the user.

That's it, nothing more.

Now you can store this information wherever you want and start to give to your logged users some superpowers 😉