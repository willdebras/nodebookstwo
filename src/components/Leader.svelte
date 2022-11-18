<script>
    import LeaderLine from 'leader-line';
    import { onMount } from 'svelte';
  
    let startLeader, endLeader;
    onMount(() => {
      new LeaderLine(startLeader, endLeader);
    });
  </script>
  
  <div class='leaderNode' bind:this={startLeader}>start</div>
  <div class='leaderNode' bind:this={endLeader}>end</div>

  <style>

    .leaderNode {
      width:10px;
      height:10px;
    }

  </style>