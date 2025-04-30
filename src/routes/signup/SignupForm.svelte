<script lang="ts">
    import { goto } from "$app/navigation";
    import { donationService } from "$lib/services/donation-service";
    import type { User } from "$lib/types/donation-types";
    import UserCredentials from "$lib/ui/UserCredentials.svelte";
    import UserDetails from "$lib/ui/UserDetails.svelte";
  
    let firstName = $state("");
    let lastName = $state("");
    let email = $state("");
    let password = $state("");
    let message = $state("");
  
    async function signup() {
      const user: User = {
        firstName: firstName,
        lastName: lastName,
        email: email,
        password: password
      }
      let success = await donationService.signup(user);
      if (success) {
        console.log(`You are signing up ${firstName} ${lastName} ${email} and ${password}`)
        goto("/login");
      } else {
        message = "Error Trying to sign up";
      }
    }
  </script>
  
  <div class="box">
    <UserDetails bind:firstName bind:lastName />
    <UserCredentials bind:email bind:password />
    <button onclick={() => signup()} class="button is-success is-fullwidth has-text-white">Sign Up</button>
    <p class="has-text-centered">
      Already have an account? <a href="/login" data-cy="login-redirect">Login Here</a>
    </p>
  </div>
  


  