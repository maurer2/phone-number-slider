<script lang="ts">
  const smallestNumber = 1;
  const largestNumber = 999_999_999;
  const numberFormatter = Intl.NumberFormat('en-GB', {
    minimumIntegerDigits: 9,
    maximumFractionDigits: 0,
    useGrouping: true,
  });
  // const list = Array(100_000);
  const containerWidth = Math.trunc(largestNumber * 1000); // 16px thumb slider

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

  const handleSubmit = (event: SubmitEvent) => {
    event.preventDefault();
  };

  let phoneNumber = $state(0);
</script>

<main class="container m-auto bg-[#BDE0FE] h-dvh p-4">
  <form aria-label="Contact form" onsubmit={handleSubmit}>
    <div class="grid gap-4 grid-cols-[auto_1fr] mb-8">
      <label for="phone-number">Phone number: </label>
      <div class="overflow-x-scroll grow-1">
        <input
          type="range"
          id="phone-number"
          name="phone-number"
          list="markers"
          style:width="{containerWidth}px"
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

    <button type="button">Submit</button>
  </form>
</main>
