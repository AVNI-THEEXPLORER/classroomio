<script>
  import CloseButton from '$lib/components/Buttons/Close/index.svelte';
  import TextField from '$lib/components/Form/TextField.svelte';
  import { PageNav } from '$lib/components/Page';
  import Vote from '$lib/components/Vote/index.svelte';
  import CheckmarkOutline from 'carbon-icons-svelte/lib/CheckmarkOutline.svelte';

  export let handleClose;
  let bodyRef;
  let question;

  let questions = [
    {
      id: 1,
      name: 'Evgeniy',
      question: "I'am looking at this and it looks great",
      vote: 1,
      answered: false
    },
    {
      id: 2,
      name: 'Sergey Semko',
      question:
        'Thanks for the feedback, what do you think about my website. https://rotimibest.com',
      vote: 4,
      answered: false
    },
    {
      id: 3,
      name: 'Vitalii Marushko',
      question: 'Всем привет',
      vote: 20,
      answered: false
    },
    {
      id: 4,
      name: 'Natasha',
      question: 'Привет Виталий',
      vote: 0,
      answered: false
    }
  ];

  function handleSend() {
    if (!question) {
      return;
    }

    questions = [
      ...questions,
      {
        id: questions.length,
        name: 'You',
        question,
        vote: 0,
        answered: false
      }
    ];

    question = null;

    setTimeout(() => {
      bodyRef.scrollTo({
        top: bodyRef.scrollHeight
      });
    }, 100);
  }

  function handleMarkAnswered(id) {
    return () => {
      questions = questions
        .map((question) => {
          if (question.id === id) {
            question.answered = !question.answered;
          }

          return question;
        })
        .sort((a, b) => Number(a.answered) - Number(b.answered));
    };
  }

  function handleKeyDown(e) {
    if (e.key === 'Enter') {
      handleSend();
    }
  }
</script>

<PageNav title="Q/A" overidableStyle="padding: 0 10px">
  <div slot="widget">
    <CloseButton onClick={handleClose} />
  </div>
</PageNav>

<div bind:this={bodyRef} class="body">
  {#each questions as question}
    <div class="mb-2 ml-2 mt-2 flex items-start">
      <Vote bind:value={question.vote} />
      <div class="ml-2 flex-grow">
        <p class="text-sm font-bold dark:text-white">{question.name}</p>
        <article class="prose prose-sm sm:prose">
          {question.question}
        </article>
      </div>
      <button class={`${question.answered && 'active'}`} on:click={handleMarkAnswered(question.id)}>
        <CheckmarkOutline size={20} class="carbon-icon dark:text-white" />
      </button>
    </div>
  {/each}
</div>

<div class="footer">
  <TextField placeholder="Say something" bind:value={question} onKeyDown={handleKeyDown} />
</div>

<style lang="scss">
  .body {
    overflow: auto;
    height: 80%;

    & button {
      display: none;
    }
    & div:hover button,
    & button.active {
      display: block;
    }
  }
  .footer {
    position: absolute;
    bottom: 0px;
    width: 100%;
  }
</style>
