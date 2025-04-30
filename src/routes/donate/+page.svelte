<!-- <script lang="ts">
  import { loggedInUser, subTitle } from "$lib/runes.svelte";
  import { donationService } from "$lib/services/donation-service";
  import Card from "$lib/ui/Card.svelte";
  import { onMount } from "svelte";
  import DonateForm from "./DonateForm.svelte";
  import type { Candidate } from "$lib/types/donation-types";

  subTitle.text = "Make a Donation";
  let candidateList: Candidate[] = [];

  /* The onMount function will be called once when this page is loaded.
   It will request the list of candidates from the api, storing them in the 
   candidateList array. This is, in turn, passed to the DonateForm component. */

  onMount(async () => {
    candidateList = await donationService.getCandidates(loggedInUser.token);
  });
</script>

<Card title="Please Donate">
  <DonateForm {candidateList} />
</Card> -->

<!-- <script lang="ts">
  import { loggedInUser, subTitle } from "$lib/runes.svelte";
  import { donationService } from "$lib/services/donation-service";
  import Card from "$lib/ui/Card.svelte";
  import { onMount } from "svelte";
  import DonateForm from "./DonateForm.svelte";
  import type { Candidate, Donation } from "$lib/types/donation-types";
  import DonationList from "$lib/ui/DonationList.svelte";

  subTitle.text = "Make a Donation";
  let candidateList: Candidate[] = [];
  let donations: Donation[] = [];

  /* The onMount function will be called once when this page is loaded.
   It will request the list of candidates from the api, storing them in the 
   candidateList array. This is, in turn, passed to the DonateForm component. */

  onMount(async () => {
    candidateList = await donationService.getCandidates(loggedInUser.token);
    donations = await donationService.getDonations(loggedInUser.token);
  });
</script>

<div class="columns">
  <div class="column">
    <Card title="Donations to Date">
      <DonationList {donations} />
    </Card>
  </div>
  <div class="column">
    <Card title="Please Donate">
      <DonateForm {candidateList} />
    </Card>
  </div>
</div> -->

<script lang="ts">
  import { curentDataSets, loggedInUser, subTitle } from "$lib/runes.svelte";
  import Card from "$lib/ui/Card.svelte";
  import DonateForm from "./DonateForm.svelte";
  // @ts-ignore
  import Chart from "svelte-frappe-charts";
  import DonationList from "$lib/ui/DonationList.svelte";
  import LeafletMap from "$lib/ui/LeafletMap.svelte";
  import { onMount } from "svelte";
  import type { Donation } from "$lib/types/donation-types";
  import { donationService } from "$lib/services/donation-service";
  import { refreshDonationMap } from "$lib/services/donation-utils";

  subTitle.text = "Make a Donation";
  let map: LeafletMap;

  // onMount(async () => {
  //   const donations = await donationService.getDonations(loggedInUser.token);
  //   donations.forEach((donation: Donation) => {
  //     if (typeof donation.candidate !== "string") {
  //       const popup = `${donation.candidate.firstName} ${donation.candidate.lastName}: €${donation.amount}`;
  //       map.addMarker(donation.lat, donation.lng, popup);
  //     }
  //   });
  //   const lastDonation = donations[donations.length - 1];
  //   if (lastDonation) map.moveTo(lastDonation.lat, lastDonation.lng);
  // });

  onMount(async () => {
    await refreshDonationMap(map);
  });

  function donationMade(donation: Donation) {
    map.addMarker(donation.lat, donation.lng, "");
    map.moveTo(donation.lat, donation.lng);
  }
</script>

<div class="columns">
  <div class="column">
    <Card title="Donations to Date">
      <LeafletMap height={60} bind:this={map} />
    </Card>
  </div>
  <div class="column">
    <Card title="Please Donate">
      <DonateForm donationEvent={donationMade} />
    </Card>
  </div>
</div>
<div class="columns">
  <div class="column">
    <Card title="Donations to Date">
      <Chart data={curentDataSets.donationsByCandidate} type="bar" />
    </Card>
  </div>
  <div class="column">
    <Card title="Please Donate">
      <DonationList />
    </Card>
  </div>
</div>
