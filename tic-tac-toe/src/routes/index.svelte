<script type="ts">
	let arrayNumber = [
		{ clicked: 0, number: 1 },
		{ clicked: 0, number: 2 },
		{ clicked: 0, number: 3 },
		{ clicked: 0, number: 4 },
		{ clicked: 0, number: 5 },
		{ clicked: 0, number: 6 },
		{ clicked: 0, number: 7 },
		{ clicked: 0, number: 8 },
		{ clicked: 0, number: 9 }
	];
	let count = 0;
	let gameEnd = '';
	let gameStatus = '';
	let turn = 'Player 1';

	let ticTacToe = '';

	function checkTTT(player: string) {
		let checkNumber = 0;
		player === 'Player 1'
			? (checkNumber = 1)
			: player === 'Player 2'
			? (checkNumber = 2)
			: (checkNumber = 3);

		if (arrayNumber[0].clicked == checkNumber) {
			if (
				arrayNumber[0].clicked == checkNumber &&
				arrayNumber[1].clicked == checkNumber &&
				arrayNumber[2].clicked == checkNumber
			) {
				ticTacToe = player + ' 123';
				gameEnd = player + ' Tic Tac Toe';
			} else if (
				arrayNumber[0].clicked == checkNumber &&
				arrayNumber[4].clicked == checkNumber &&
				arrayNumber[8].clicked == checkNumber
			) {
				ticTacToe = player + ' 159';
				gameEnd = player + ' Tic Tac Toe';
			} else if (
				arrayNumber[0].clicked == checkNumber &&
				arrayNumber[3].clicked == checkNumber &&
				arrayNumber[6].clicked == checkNumber
			) {
				ticTacToe = player + ' 147';
				gameEnd = player + ' Tic Tac Toe';
			}
		}
		if (
			arrayNumber[1].clicked == checkNumber &&
			arrayNumber[4].clicked == checkNumber &&
			arrayNumber[7].clicked == checkNumber
		) {
			ticTacToe = player + ' 258';
			gameEnd = player + ' Tic Tac Toe';
		} else if (
			arrayNumber[2].clicked == checkNumber &&
			arrayNumber[4].clicked == checkNumber &&
			arrayNumber[6].clicked == checkNumber
		) {
			ticTacToe = player + ' 357';
			gameEnd = player + ' Tic Tac Toe';
		} else if (
			arrayNumber[2].clicked == checkNumber &&
			arrayNumber[5].clicked == checkNumber &&
			arrayNumber[8].clicked == checkNumber
		) {
			ticTacToe = player + ' 369';
			gameEnd = player + ' Tic Tac Toe';
		} else if (
			arrayNumber[3].clicked == checkNumber &&
			arrayNumber[4].clicked == checkNumber &&
			arrayNumber[5].clicked == checkNumber
		) {
			ticTacToe = player + ' 456';
			gameEnd = player + ' Tic Tac Toe';
		} else if (
			arrayNumber[6].clicked == checkNumber &&
			arrayNumber[7].clicked == checkNumber &&
			arrayNumber[8].clicked == checkNumber
		) {
			ticTacToe = player + ' 789';
			gameEnd = player + ' Tic Tac Toe';
		}
	}

	$: if (turn === 'Computer') {
		setTimeout(computerGuess, 500);
	}

	function computerGuess() {
		console.log('Computer Guess');
		let guessOptions: number[] = [];

		arrayNumber.forEach((square) => {
			square.clicked == 0 ? guessOptions.push(square.number) : '';
		});
		console.log(guessOptions.toString());

		if (count < 2) {
			if (guessOptions.includes(5)) {
				arrayNumber[4].clicked = 3;
			} else {
				var guessCorners = [0, 2, 6, 8];
				let guess = guessCorners[Math.floor(Math.random() * guessCorners.length)];
				arrayNumber[guess - 1].clicked = 3;
			}
		} else {
			var guessNumber = guessOptions[Math.floor(Math.random() * guessOptions.length)];
			console.log('Guess number:' + guessNumber);
			arrayNumber[guessNumber - 1].clicked = 3;
		}
		if (count < 9) {
			count = count + 1;
			checkTTT('Computer');
			turn = 'Player 1';
		}
	}
</script>

<h1 class="flex mx-auto justify-center text-2xl font-bold">Tic Tac Toe</h1>
<div class="h-96 w-96 justify-center mx-auto my-5 ">
	<div class="grid grid-cols-3">
		<!--Each Square 1-9 -->
		{#each arrayNumber as square}
			<button
				class={`box-border h-32 w-32 p-4 border-2 rounded-md ${
					square.clicked == 1
						? 'bg-blue-500'
						: square.clicked == 2
						? 'bg-red-500'
						: square.clicked == 3
						? 'bg-purple-400'
						: gameStatus !== '' && gameEnd === ''
						? turn === 'Player 1'
							? 'hover:bg-blue-300'
							: 'hover:bg-red-300'
						: 'pointer-events-none'
				}`}
				on:click={() => {
					if (gameStatus === '') {
						alert('Select either Two Player or Vs Computer to start');
					} else if (gameEnd !== '') {
						alert(
							'Game Finished : ' +
								gameEnd +
								'! Select Reset to clear the board and start a new game!'
						);
					} else if (arrayNumber[square.number - 1].clicked == 0) {
						if (gameStatus === 'TwoPlayer') {
							if (turn === 'Player 1') {
								arrayNumber[square.number - 1].clicked = 1;
								turn = 'Player 2';
							} else {
								arrayNumber[square.number - 1].clicked = 2;
								turn = 'Player 1';
							}
							checkTTT('Player 1');
							checkTTT('Player 2');
							count = count + 1;
							if (count == 9 && gameEnd === '') {
								gameEnd = 'Cat Game';
							}
						} else if (gameStatus === 'VsComputer') {
							if (turn === 'Player 1') {
								arrayNumber[square.number - 1].clicked = 1;
								count = count + 1;
							}
							checkTTT('Player 1');
							if ((gameEnd === '')) {
								checkTTT('Computer');
							}

							if (count < 9) {
								turn = 'Computer';
							}
							if (count >  8){
								if(gameEnd === ""){
									gameEnd = 'Cat Game';
								}
							
							
								
							}
						}
					}
				}}
			>
				<h1 class="text-3xl">
					{square.clicked == 0 ? '' : square.clicked == 1 ? 'X' : 'O'}
				</h1></button
			>
		{/each}
	</div>
</div>

<h1 class="flex justify-center">{ticTacToe}</h1>
<h1>{gameEnd === '' ? '' : gameEnd}</h1>
<h1 class="flex justify-center">
	{gameEnd !== '' ? '' : gameStatus !== '' ? turn + ' Turn' : ''}
</h1>
<h1>Count = {count}</h1>
<h1 class="flex justify-center my-3">
	{gameStatus === '' ? 'Select Either Two Player Game or Vs Computer to Start' : ''}
</h1>

<div class="flex mx-auto justify-center space-x-5">
	{#if gameStatus !== 'VsComputer'}
		<!--Two Player Button -->
		<button
			on:click={() => {
				gameStatus = 'TwoPlayer';
			}}
			class={`border-2 border-black p-4 hover:bg-orange-300 rounded-lg ${
				gameStatus === 'VsComputer'
					? 'invisible'
					: gameStatus === 'TwoPlayer'
					? 'bg-orange-600 text-white font-semibold'
					: ''
			}`}
			>Two Players
		</button>
	{/if}

	<!--Reset Button -->
	{#if gameStatus !== ''}
		<button
			on:click={() => {
				arrayNumber = [
					{ clicked: 0, number: 1 },
					{ clicked: 0, number: 2 },
					{ clicked: 0, number: 3 },
					{ clicked: 0, number: 4 },
					{ clicked: 0, number: 5 },
					{ clicked: 0, number: 6 },
					{ clicked: 0, number: 7 },
					{ clicked: 0, number: 8 },
					{ clicked: 0, number: 9 }
				];
				ticTacToe = '';
				gameStatus = '';
				turn = 'Player 1';

				count = 0;
				gameEnd = '';
			}}
			class={`border-2 border-black p-4 hover:bg-cyan-300 rounded-lg ${
				gameEnd !== '' ? 'bg-yellow-200' : ''
			} `}>Reset</button
		>
	{/if}

	{#if gameStatus !== 'TwoPlayer'}
		<!--Vs Computer Button -->
		<button
			on:click={() => {
				gameStatus = 'VsComputer';
			}}
			class={`border-2 border-black p-4 hover:bg-purple-300 rounded-lg ${
				gameStatus === 'TwoPlayer'
					? 'invisible'
					: gameStatus === 'VsComputer'
					? 'bg-purple-600 text-white font-semibold'
					: ''
			}  `}
			>Vs Computer
		</button>
	{/if}
</div>

<style>
</style>
