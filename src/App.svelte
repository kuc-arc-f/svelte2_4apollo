<script>
export let name;
import { ApolloClient, InMemoryCache ,gql} from "@apollo/client";
import { setClient ,query } from "svelte-apollo";

const client = new ApolloClient({
	uri: "http://localhost:4000/graphql",
	cache: new InMemoryCache(),
});
setClient(client);

const GET_TASKS = gql`
  query {
    tasks {
      id
      title
    }
  }
`;
const get_items = async function(){
  const data = await client.query({
    query: GET_TASKS, fetchPolicy: "network-only"
  })
  console.log(data)
}
get_items()

//
const get_gql_add= function(title){
  return gql`
    mutation {
      addTask( title: "${title}") {
        id
        title
      }                
    }      
  `   
}
async function handleClick() {
console.log('clicked')
  await add_item()
}
async function add_item(){
  try {
    const result = await client.mutate({mutation: get_gql_add("hoge")}) 
console.log(result)   
  } catch (error) {
    console.error(error);
  }    
}
</script>

<main>
	<h1>Hello {name}!</h1>
	<p>Visit the <a href="https://svelte.dev/tutorial">Svelte tutorial</a> to learn how to build Svelte apps.
  </p>
  <hr />
  <button on:click={handleClick} class="btn btn-primary">Add</button>
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>