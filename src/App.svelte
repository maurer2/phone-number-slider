<script lang="ts">
  const numberFormatter = Intl.NumberFormat('en-GB', {
    minimumIntegerDigits: 9,
    maximumFractionDigits: 0,
    useGrouping: true,
  });

  const handleSubmit = (event: SubmitEvent) => {
    event.preventDefault();
  };

  const formatNumberWithSeparators = (value: number) => {
    const numberParts = numberFormatter.formatToParts(value);
    const numberPartsWithHyphens = numberParts.flatMap((entry) => {
      if (entry.type === 'group') {
        return '-';
      }
      if (entry.type === 'integer') {
        return entry.value;
      }
      return [];
    });
    const numberFinal = numberPartsWithHyphens.join('');

    return numberFinal;
  };

  let phoneNumber = $state(555_123_123);

  const smallestNumber = 1;
  const largestNumber = 999_999_999;
  // const list = Array(100_000);
</script>

<main class="container m-auto bg-[#BDE0FE] h-dvh">
  <form aria-label="Contact form" onsubmit={handleSubmit}>
    <div class="grid gap-4 grid-cols-[auto_1fr] p-4">
      <label for="phone-number">Phone number: </label>
      <div class="overflow-visible grow-1">
        <input
          type="range"
          id="phone-number"
          name="phone-number"
          list="markers"
          class="w-full"
          min={smallestNumber}
          max={largestNumber}
          bind:value={phoneNumber}
          step="1"
        />
        <!-- <datalist id="markers">
          {#each list as _, index}
            <option>{index}</option>
          {/each}
        </datalist> -->
      </div>
      <p class="col-span-full">
        Current number: <output for="phone-number">{formatNumberWithSeparators(phoneNumber)}</output
        >
      </p>
    </div>

    <hr />

    <button type="button">Submit</button>
  </form>
</main>
