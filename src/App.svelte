<script>
	import { afterUpdate, onMount, setContext} from "svelte";

	// components
	import Navbar from "./Navbar.svelte";
	import Totals from "./Totals.svelte";
	import ExpenseForm from "./ExpenseForm.svelte"
	import ExpensesList from "./ExpensesList.svelte";
	// import Github from "./Github.svelte";
	import GithubAwait from "./GithubAwait.svelte";
	import Modal from "./Modal.svelte";

	// data
	// import expensesData from "./expenses";

	// variables 
	let expenses = []
	// console.log(expensesData);

	// set editing variables
	let setName = "";
	let setAmount = null;
	let setId = null;

	// toggle form variables
	let isFormOpen = false;

	// reactive 
	$: isEditing = setId ? true : false;

	$: total = expenses.reduce((acc, curr) => {
		return ( acc += curr.amount)
	}, 0);

	// functions
	function showForm() {
		isFormOpen = true;
	}

	function hideForm() {
		isFormOpen = false;
		SetName = "";
		setAmount = null;
		setI = null;
	}

	const removeExpense = (id) => {
		expenses = expenses.filter(item => item.id !== id);
		// setLocalStorage()
	}

	const addExpense = ({name, amount}) => {
		let expense = {id: Math.random() * Date.now(), name, amount};
		expenses = [expense, ...expenses];
		// setLocalStorage()
	}

	const setModifiedExpense = (id) => {
		let expense = expenses.find(item => item.id === id);
		setId = expense.id;
		setName = expense.name;
		setAmount = expense.amount;
		showForm()
	}

	const editExpense = ({name, amount}) => {
		expenses = expenses.map(item => {
			return item.id === setId ? {...item, name, amount} : {...item}
		})
		setId = null;
		setAmount = null;
		setName = "";
		// setLocalStorage();
	}
	//context 
	setContext("remove", removeExpense);
	setContext("modify", setModifiedExpense);

	const clearExpenses = () => {
		expenses = [];
		// setLocalStorage();
	}

	// local Storage
	function setLocalStorage() {
		localStorage.setItem("expenses", JSON.stringify(expenses));
	}

	// here we're grabbing localStorage and put to expenses array
	onMount(() => {
		expenses = localStorage.getItem("expenses") ? JSON.parse(localStorage.getItem("expenses")) : []
	})

	// utilize to run after every mounts and update 
	// but carefully what we are running inside like calling API 
	// or function that take more time
	afterUpdate(() => {
		console.log("after update");
		setLocalStorage();
	})
</script>


<Navbar {showForm}/>
<main class="content">
	<!-- <GithubAwait /> -->
	{#if isFormOpen}
	<Modal>
		<ExpenseForm 
			{addExpense} 
			name={setName} 
			amount={setAmount}  
			{isEditing} 
			{editExpense}
			{hideForm}
		/>
	</Modal>
	{/if}

	<Totals title="total expenses" {total}/>
	<ExpensesList {expenses} {removeExpense} />
	<button type="button" class="btn btn-primary btn-block" on:click={clearExpenses}>Clear Expenses</button>
</main>

