<script lang="ts">
  import pkg from '@mobiscroll/javascript';
  const {eventcalendar, toast, util} = pkg;
  import { onDestroy, onMount } from 'svelte';

  import '@mobiscroll/javascript/dist/css/mobiscroll.min.css';
  import {browser} from "$app/environment";

  let eventCalendarInstance;
  onMount(() => {
    console.log("Component mounted")
    eventCalendarInstance = eventcalendar('#calendar-2', {
      theme: 'ios',
      themeVariant: 'light',
      clickToCreate: true,
      dragToCreate: true,
      dragToMove: true,
      dragToResize: true,
      eventDelete: true,
      view: {
        schedule: { type: 'week' },
      },
      onEventClick: function (event) {
        toast({
          message: event.event.title,
        });
      },
    });

    util.http.getJson(
        'https://trial.mobiscroll.com/events/?vers=5',
        function (events) {
          eventCalendarInstance.setEvents(events);
        },
        'jsonp',
    );
  });

  onDestroy( () => {
    if (browser) {
      console.log("Component removed")
      eventCalendarInstance.destroy()
    }
  });
</script>

<div id="calendar-2"></div>
