<script context="module" lang="ts">
  export const prerender = true;
</script>

<script lang="ts">
  import { logout } from '../lib/authentication';
  import { current } from '../lib/stores';

  import Button from '../components/Button.svelte';
  import Image from '../components/Image.svelte';
</script>

<section>
  {#if $current?.user == undefined}
    <div class="prompt">
      <h1>One sec</h1>
    </div>
  {:else if $current.playback?.is_playing}
    <div class="music">
      <div class="playback">
        <img src={$current.imageUri} alt={$current.song?.name} />
        <div class="song">
          <h1 class="fat">{$current.song?.name}</h1>
          <h2>{$current.song?.artists[0].name}</h2>
          <div class="progress-bar">
            {#each Array($current?.analysis?.sections?.length) as _, i}
              <div
                class="progress-chapter"
                style="
                height: {-1/$current.analysis?.sections[i].loudness * 20}px;
                background-color: {$current.colors?.DarkMuted.hex}; 
                width: {($current?.analysis?.sections[i].duration /
                  $current?.song?.duration_ms) *
                  100000}%;"
              >
                <div
                  class="progress"
                  style="
                  background-color: {$current.colors?.LightVibrant.hex}; 
                  width: {$current.elapsed > $current.analysis?.sections[i].start ? (($current.elapsed - $current.analysis?.sections[i].start) / $current.analysis?.sections[i].duration * 100) : 0}%;"
                />
              </div>
            {/each}
          </div>
        </div>
      </div>
      <!-- <div class="colors">
        <div
          class="color"
          style="background-color: {$current.colors?.LightMuted.hex};"
        >
          <h1>Light Muted</h1>
        </div>
        <div
          class="color"
          style="background-color: {$current.colors?.Muted.hex};"
        >
          <h1>Muted</h1>
        </div>
        <div
          class="color"
          style="background-color: {$current.colors?.DarkMuted.hex};"
        >
          <h1>Dark Muted</h1>
        </div>

        <div
          class="color"
          style="background-color: {$current.colors?.LightVibrant.hex};"
        >
          <h1>Light Vibrant</h1>
        </div>
        <div
          class="color"
          style="background-color: {$current.colors?.Vibrant.hex};"
        >
          <h1>Vibrant</h1>
        </div>
        <div
          class="color"
          style="background-color: {$current.colors?.DarkVibrant.hex};"
        >
          <h1>Dark Vibrant</h1>
        </div>
      </div> -->
    </div>
  {:else}
    <div class="no-music">
      <Image source={$current.imageUri} width={400} height={400} />
      <h2>You're currently not playing any music</h2>
    </div>
  {/if}
</section>

<style lang="scss">
  section {
    display: flex;
    flex-grow: 1;
    opacity: 0.8;

    .prompt {
      display: flex;
      flex-grow: 1;
      align-items: center;
      justify-content: center;
    }

    .no-music {
      flex-grow: 1;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
    }

    .music {
      flex-grow: 1;
      display: flex;
      align-items: flex-end;
      justify-content: start;
      justify-content: space-between;

      .colors {
        margin: 2rem;
        display: flex;
        align-items: center;
        flex-direction: column;

        .color {
          display: flex;
          flex-direction: column;
          align-items: center;
          justify-content: center;
          text-align: center;
          border-radius: 10px;
          width: 90px;
          height: 90px;
          margin: 0.5rem;
          transition: background-color 1s ease;
          box-shadow: 0 0 100px 10px rgba(0, 0, 0, 0.25);

          h1 {
            margin: 0;
            font-size: 1rem;
          }
        }
      }

      .playback {
        display: flex;
        align-items: center;
        margin: 2rem;
        flex-grow: 1;

        img {
          width: 150px;
          height: 150px;
          margin-right: 20px;
          box-shadow: 0 0 100px 10px rgba(0, 0, 0, 0.25);
          border-radius: 10px;
        }

        .song {
          h1,
          h2 {
            margin: 0;
          }

          display: flex;
          flex-grow: 1;
          flex-direction: column;
          justify-content: space-around;

          .progress-bar {
            width: 100%;
            border-radius: 100px;
            margin-top: 1rem;
            margin-bottom: -1rem;
            opacity: 0.7;
            transition: background-color 2s ease;
            display: flex;
            align-items: center;

            .progress-chapter {
              margin: 0px 1.5px;
              border-radius: 100px;
              min-height: 2.5px;
              max-height: 10px;
              transition: all 200ms ease;
              box-shadow: 0 0 100px 10px rgba(0, 0, 0, 0.25);

              .progress {
                width: 0%;
                height: 100%;
                border-radius: 100px;
                transition: background-color 200ms ease;
                max-width: 100%;
              }
            }
          }
        }
      }
    }
  }
</style>
