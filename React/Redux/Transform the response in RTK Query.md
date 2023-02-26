In case the response from the API does not give you a unique value that you can use as ID, or you need to do something different, you can leverage the `transformResponse` fn right inside the `endpoint` you define:
```js
const Api = createApi({
  reducerPath: "api",
  baseQuery: fetchBaseQuery({ baseUrl }),
  endpoints: (builder) => ({
    fetchPeople: builder.query({
      query: () => `path`,
      transformResponse: (returnValue) => {
	    // Here you can transform the returned value
	    // Don't forget to return it!!!
      }  
    }),
  }),
});
```