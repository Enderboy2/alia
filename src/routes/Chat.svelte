<script>
  // Array of Alia's predefined responses
  let responses = [
    "ddddddddd",
    "Enta m4 gy m3aya seka",
    "3ayel/a Fla7/a",
    "*attempts to step on your shoe*"
  ];

  // Chat state
  let messages = [];
  let userInput = "";
  let currentIndex = 0;

  // Handle user sending a message
  function handleSend() {
    if (!userInput.trim()) return; // Ignore empty input

    // Add user's message to chat
    messages = [...messages, { sender: "Me", text: userInput }];
    userInput = "";

    // Respond with Alia's message if there are responses left
    if (currentIndex < responses.length) {
      setTimeout(() => {
        messages = [...messages, { sender: "Alia", text: responses[currentIndex] }];
        currentIndex++;
      }, 500); // Simulate typing delay
    }
  }
</script>

<div class="max-w-full mx-auto mt-10  shadow-lg rounded-lg p-6">
  <h1 class="text-2xl font-bold text-center mb-4">Average conversation with HER</h1>
  <div class="space-y-4 mb-4">
    {#each messages as { sender, text }}
      <div
        class={`flex ${sender === "Me" ? "justify-end" : "justify-start"}`}
      >
        <div
          class={`px-4 py-2 rounded-lg text-sm ${
            sender === "Me"
              ? "bg-blue-500 text-white "
              : "bg-white text-black "
          }`}
        >
          <span class="font-semibold text-sm">{sender}:</span> {text}
        </div>
      </div>
    {/each}
  </div>
  <div class="flex items-center space-x-2">
    <input
      type="text"
      class="flex-grow border border-gray-300 rounded-lg p-1 max-w-[80%] text-sm text-black"
      placeholder="Type your message here..."
      bind:value={userInput}
      disabled={currentIndex >= responses.length}
    />
    <button
      class="bg-blue-500 text-white p-2 text-sm rounded-lg hover:bg-blue-600 disabled:bg-gray-300"
      on:click={handleSend}
      disabled={currentIndex >= responses.length}
    >
      Send
    </button>
  </div>
</div>
