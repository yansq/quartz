
`npx` is pre-Bundled with [[npm]] since version 5.2.0.

`npx` can run a locally installed package easily:

```bash
npx your-package
```

However, the major advantage of npx is the ability to execute a package that wasn't previously installed:

```bash
npx cowsay wow	
```

The `cowsay` package will be deleted once the project is started.
So, when we want to try something new without installing it permanently, using `npx` is a good choice.

`npx` also can be used to test different versions of packages. For example, if we want to test the upcoming version of `create-react-app`, we can just:

```bash
npm v create-react-app
# dist-tags:
# canary: 3.3.0-next.38  latest: 5.0.1          next: 5.1.0-next.14

npx create-react-app@next sandbox
```

`npx` will temporarily install the next version of `create-react-app`, and then it’ll execute to scaffold the app and install its dependencies.

Once installed, we can navigate to the app like this:

```bash
cd sandbox
```

and then start it:
```bash
npm start
```

## References

- [npm vs npx — What’s the Difference?](https://www.freecodecamp.org/news/npm-vs-npx-whats-the-difference/)
- [npx 是什麼? 跟 npm 差在哪?](https://medium.com/itsems-frontend/whats-npx-e83400efe7f8)