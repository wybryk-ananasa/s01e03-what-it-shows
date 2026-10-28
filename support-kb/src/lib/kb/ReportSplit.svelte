<script lang="ts">
  import reports from '#lib/data/reports-4471.json';
  import notes from '#lib/data/desk-notes.json';
  import { causeOf, uniqueById, type Report } from '#lib/kb/causes';
  import { tenantOf } from '#lib/kb/locales';

  const merged = uniqueById(reports as Report[]).filter((r) => r.mergedInto === 4471);
  const oneDay = new Set(notes.oneDayMove);
  const counts = merged.reduce((n, r) => {
    if (oneDay.has(r.id)) n.oneDay++;
    const cause = causeOf(r, tenantOf(r));
    if (cause === 'parser') n.parser++;
    else if (cause === 'hint') n.hint++;
    else n.unplaced++;
    return n;
  }, { parser: 0, hint: 0, oneDay: 0, unplaced: 0 });
</script>

<p>{merged.length} unique reports merged into #4471 in the export.</p>
<table>
  <thead><tr><th>Problem</th><th>Reports</th><th>Ticket</th></tr></thead>
  <tbody>
    <tr><td>Day-first dates parsed month-first</td><td>{counts.parser}</td><td><a href="https://github.com/wybryk-ananasa/s01e03-what-it-shows/issues/3">#3</a></td></tr>
    <tr><td>Booking-v3 hint disagrees with the tenant's date format</td><td>{counts.hint}</td><td><a href="https://github.com/wybryk-ananasa/s01e03-what-it-shows/issues/1">#1</a></td></tr>
    {#if counts.unplaced}<tr><td>Unplaced: investigate</td><td>{counts.unplaced}</td><td>Not yet filed</td></tr>{/if}
  </tbody>
</table>
<p>{counts.oneDay} reports read as a one-day move; they are included in the parser count by the export's cause classification.</p>
