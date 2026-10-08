# LIST
<br>
Preet More
<br>
New Assignment
<br>
#include &lt;iostream&gt;
#include &lt;string&gt;
using namespace std;
struct Appointment {
int start, end;
string name;
Appointment* prev;
Appointment* next;
};
class Schedule {
private:
Appointment* head;
int toMinutes(int h, int m) {
return h * 60 + m;
}
void showTime(int t) {
cout &lt;&lt; (t / 60) &lt;&lt; &quot;:&quot; &lt;&lt; (t % 60 &lt; 10 ? &quot;0&quot; : &quot;&quot;) &lt;&lt; t % 60;
}
bool overlap(int s, int e) {
for (Appointment* p = head; p; p = p-&gt;next) {
if (!(e &lt;= p-&gt;start || s &gt;= p-&gt;end)) {
return true;
}
if (p-&gt;start &gt; e) {
break;
}
}
return false;
}
public:
Schedule() : head(nullptr) { }
void bookAppointment() {
int sh, sm, eh, em;
string name;
cout &lt;&lt; &quot;Enter name: &quot;;

cin &gt;&gt; name;
cout &lt;&lt; &quot;Enter start time (hh mm): &quot;;
cin &gt;&gt; sh &gt;&gt; sm;
cout &lt;&lt; &quot;Enter end time (hh mm): &quot;;
cin &gt;&gt; eh &gt;&gt; em;
int s = toMinutes(sh, sm);
int e = toMinutes(eh, em);
if (s &gt;= e || s &lt; 540 || e &gt; 1020) {
cout &lt;&lt; &quot;Invalid time! Working hours: 9:00 - 17:00.\n&quot;;
return;
}
if (e - s &lt; 30) {
cout &lt;&lt; &quot;Minimum slot is 30 minutes!\n&quot;;
return;
}
if (overlap(s, e)) {
cout &lt;&lt; &quot;Slot not free!\n&quot;;
return;
}
Appointment* n = new Appointment{s, e, name, nullptr, nullptr};
if (!head) {
head = n;
return;
}
Appointment* p = head;
while (p-&gt;next &amp;&amp; p-&gt;next-&gt;start &lt; s) {
p = p-&gt;next;
}
n-&gt;next = p-&gt;next;
if (p-&gt;next) {
p-&gt;next-&gt;prev = n;
}
n-&gt;prev = p;
p-&gt;next = n;
cout &lt;&lt; &quot;Appointment booked!\n&quot;;
}
void displayAppointments() {
if (!head) {
cout &lt;&lt; &quot;No appointments scheduled.\n&quot;;
return;
}

cout &lt;&lt; &quot;\nAppointments:\n&quot;;
for (Appointment* p = head; p; p = p-&gt;next) {
showTime(p-&gt;start);
cout &lt;&lt; &quot; - &quot;;
showTime(p-&gt;end);
cout &lt;&lt; &quot; : &quot; &lt;&lt; p-&gt;name &lt;&lt; &quot;\n&quot;;
}
}
void displayFreeSlots() {
cout &lt;&lt; &quot;\nFree 30-Minute Slots:\n&quot;;
if (!head) {
cout &lt;&lt; &quot;9:00 - 17:00\n&quot;;
return;
}
int prevEnd = 540;
for (Appointment* p = head; p; p = p-&gt;next) {
if (p-&gt;start &gt; prevEnd) {
showTime(prevEnd);
cout &lt;&lt; &quot; - &quot;;
showTime(p-&gt;start);
cout &lt;&lt; &quot;\n&quot;;
}
prevEnd = p-&gt;end;
}
if (prevEnd &lt; 1020) {
showTime(prevEnd);
cout &lt;&lt; &quot; - 17:00\n&quot;;
}
}
void cancelAppointment() {
int h, m;
cout &lt;&lt; &quot;Enter start time (hh mm): &quot;;
cin &gt;&gt; h &gt;&gt; m;
int t = toMinutes(h, m);
Appointment* p = head;
while (p &amp;&amp; p-&gt;start != t) {
p = p-&gt;next;
}
if (!p) {
cout &lt;&lt; &quot;Appointment not found!\n&quot;;
return;
}
if (p-&gt;prev) {

p-&gt;prev-&gt;next = p-&gt;next;
} else {
head = p-&gt;next;
}
if (p-&gt;next) {
p-&gt;next-&gt;prev = p-&gt;prev;
}
delete p;
cout &lt;&lt; &quot;Appointment cancelled successfully.\n&quot;;
}
void sortByData() {
if (!head) return;
for (Appointment* i = head; i &amp;&amp; i-&gt;next; i = i-&gt;next) {
for (Appointment* j = head; j &amp;&amp; j-&gt;next; j = j-&gt;next) {
if (j-&gt;start &gt; j-&gt;next-&gt;start) {
swap(j-&gt;start, j-&gt;next-&gt;start);
swap(j-&gt;end, j-&gt;next-&gt;end);
swap(j-&gt;name, j-&gt;next-&gt;name);
}
}
}
cout &lt;&lt; &quot;Appointments sorted by data.\n&quot;;
displayAppointments();
}
void sortByPointer() {
if (!head || !head-&gt;next) return;
Appointment* sorted = nullptr;
while (head) {
Appointment* curr = head;
head = head-&gt;next;
curr-&gt;prev = curr-&gt;next = nullptr;
if (!sorted || curr-&gt;start &lt; sorted-&gt;start) {
curr-&gt;next = sorted;
if (sorted) sorted-&gt;prev = curr;
sorted = curr;
} else {
Appointment* p = sorted;
while (p-&gt;next &amp;&amp; p-&gt;next-&gt;start &lt; curr-&gt;start) {
p = p-&gt;next;
}
curr-&gt;next = p-&gt;next;
if (p-&gt;next) p-&gt;next-&gt;prev = curr;
p-&gt;next = curr;
curr-&gt;prev = p;

}
}
head = sorted;
cout &lt;&lt; &quot;Appointments sorted by pointer.\n&quot;;
displayAppointments();
}
};
int main() {
Schedule s;
int choice;
do {
cout &lt;&lt; &quot;\n===== APPOINTMENT MANAGEMENT =====\n&quot;;
cout &lt;&lt; &quot;1. Book Appointment\n&quot;;
cout &lt;&lt; &quot;2. Display Appointments\n&quot;;
cout &lt;&lt; &quot;3. Display Free Slots\n&quot;;
cout &lt;&lt; &quot;4. Cancel Appointment\n&quot;;
cout &lt;&lt; &quot;5. Sort by Data\n&quot;;
cout &lt;&lt; &quot;6. Sort by Pointer\n&quot;;
cout &lt;&lt; &quot;7. Exit\n&quot;;
cout &lt;&lt; &quot;Enter choice: &quot;;
cin &gt;&gt; choice;
switch (choice) {
case 1: s.bookAppointment(); break;
case 2: s.displayAppointments(); break;
case 3: s.displayFreeSlots(); break;
case 4: s.cancelAppointment(); break;
case 5: s.sortByData(); break;
case 6: s.sortByPointer(); break;
case 7: cout &lt;&lt; &quot;Exiting program...\n&quot;; break;
default: cout &lt;&lt; &quot;Invalid choice!\n&quot;;
}
} while (choice != 7);
return 0;
}
