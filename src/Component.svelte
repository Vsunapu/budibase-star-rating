<script>
  import { getContext } from "svelte";
  const { styleable } = getContext("sdk");
  const component = getContext("component");

  export let data = { rows: [] };
  export let ratingField = "Rating";
  export let fullStar = "★";
  export let emptyStar = "✩";
  export let maxStars = 5;

  $: averageRating = calculateAverageRating(data.rows || []);

  function calculateAverageRating(rows) {
    if (!Array.isArray(rows) || rows.length === 0) {
      return 0;
    }

    const totalRating = rows.reduce((sum, item) => {
      const rating = item[ratingField];
      return typeof rating === 'number' ? sum + rating : sum;
    }, 0);

    return totalRating / rows.length;
  }

  $: starDisplay = Array(maxStars)
    .fill()
    .map((_, i) => (i < averageRating ? fullStar : emptyStar))
    .join("");
</script>

<div class="star-rating" use:styleable={$component.styles}>
  <span class="stars">{starDisplay}</span>
  <span class="average">({averageRating.toFixed(1)})</span>
</div>

<style>
  .star-rating {
    display: flex;
    align-items: center;
    gap: 8px;
  }
  .stars {
    font-size: 1.2em;
    color: #ffd700;
  }
  .average {
    color: #666;
  }
</style>