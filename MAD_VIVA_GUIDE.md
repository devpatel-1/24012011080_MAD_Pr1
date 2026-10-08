# MAD Viva Guide – Practicals 1, 2, 3, 4, 6, 7

Dev Patel · 24012011080 · Built from your own repos (Pr1, Pr2, Pr3, Pr4, Pr6, Pr7).
Each section: **what it does → files → how it flows → concepts → likely viva questions → things to be honest about.**

---

## Practical 1 – Kotlin fundamentals (repo `24012011080_MAD_Pr1`)

Plain Kotlin console programs (no Android). Each file has its own `main()`.

| File | What it shows | Concepts |
|---|---|---|
| `P11.kt` | All basic types: Int, Float, Char, String, Boolean, Double, Long, Short, Byte; `j+f` adds Int + Float | `val`, explicit types, string templates `"$x"` / `"${expr}"`, numeric widening in `+` |
| `P12.kt` | Type conversion | `toDouble()`, `toInt()` – Kotlin has **no implicit conversion**, you must call these |
| `P13.kt` | Reads student data | `readln()` (returns String), `.toInt()` |
| `P14.kt` | Even/odd | `if` **as an expression** (returns a value), `%` |
| `P15.kt` | Month number → name | `when` expression (Kotlin's switch), `else` branch |
| `P16.kt` | Calculator with add/sub/mul/div | Functions: `fun name(a: Int, b: Int): Int` |
| `P17.kt` | Factorial | **Recursion**, base case `number <= 1` |
| `P18.kt` | Arrays | `arrayOf`, `Array(5){0}`, `Array(8){index->index}`, `IntArray`, `intArrayOf`, 2D array, `contentToString()`, `contentDeepToString()`, built-in `sort()` vs manual **bubble sort**, `copyOf()`, `0..<5` range |
| `P19.kt` | ArrayList + max | `arrayListOf`, `.indices`, `maxOrNull()` |
| `P110.kt` | `Car` class | Primary constructor with `val`/`var` properties, `init {}` block, methods, list of objects, named arguments, `"-".repeat(50)` |
| `P111.kt` | `Matrix` class | **Operator overloading**: `operator fun plus / minus / times`, `override fun toString()`, `throw IllegalArgumentException` |
| `P1ex1.kt` | Swap two numbers | With a temp variable; without one using `x = x+y; y = x-y; x = x-y` |
| `P1ex2.kt` | `Product` → `Laptop` | **Inheritance**, `open class`, primary + **secondary constructors** (`constructor(...) : this(...)`), constructor call order |
| `P1ex3.kt` | `Person` → `Student` | Same idea; secondary constructors supply defaults |
| `Main.kt` | IntelliJ's template "Hello Kotlin" | – |

### Concepts you must be able to explain
- **`val` vs `var`**: `val` = read-only reference (like `final`), `var` = mutable.
- **Null safety**: `String` can't be null, `String?` can. `maxOrNull()` returns `Int?` for that reason (empty list → `null`).
- **Classes are `final` by default** – you must write `open` to inherit (`Product`, `Person`). Methods likewise need `open` to be overridden.
- **Constructor order** (inheritance): parent primary constructor and its `init` run first, then the child's `init`. Your `Laptop` output prints "Product Primary Constructor called" before "Laptop Primary Constructor called" – be ready to say why.
- **Secondary constructor must delegate** to the primary with `: this(...)`.
- **`init` block**: runs as part of the primary constructor, in order with property initialisers.
- **String templates** `$name` and `${expression}`.
- **Array vs ArrayList**: Array is fixed size; ArrayList grows (`add`).
- **`for (i in 0 until n)` vs `0..n` vs `0..<n`**: `..` includes the end; `until` and `..<` exclude it.
- **`when` vs Java `switch`**: no `break`, can be an expression, can match ranges/types.
- **Operator overloading**: `a + b` is turned into `a.plus(b)`; the function must be marked `operator`.
- **Matrix rules**: add/subtract need identical dimensions; multiply needs `cols(A) == rows(B)`; result is `rows(A) × cols(B)`. Multiplication uses 3 nested loops.
- **Bubble sort**: compare neighbours, swap if out of order; the largest value "bubbles" to the end each pass; O(n²).

### Things to be honest about / small traps
- `P16.divide` does **integer division** and crashes with `ArithmeticException` on divide-by-zero.
- `P17.factorial` returns `Int`, so it **overflows above 12!**. `Long` would fix it.
- `P111.kt`: the "Addition" demo adds `secondMatrix + secondMatrix1` (they hold identical values), not `firstMatrix`. Output is still correct (a matrix added to itself) – just know it.
- `P110.kt` prints some "Creating Car Class Object car1…" lines twice – cosmetic.
- The **README file names don't match the repo** (it says `p1.kt`, `p11.kt`… and describes e.g. "p14 = loops", but in `src/` P14 is if-else, P15 is `when`, P16 is functions, P17 is recursion, P18 is arrays, P19 is ArrayList, P110 is classes, P111 is operator overloading). If the examiner opens the README, go by what the code actually does.
- Swapping without a third variable via `x+y` can overflow for huge ints (XOR avoids it).

### Likely viva questions
1. Difference between `val` and `var`? · 2. What is a primary vs secondary constructor? · 3. Why do we write `open`? · 4. What does `init` do? · 5. What is `?` in `Int?`? · 6. `when` vs `if`? · 7. How is Kotlin different from Java (null safety, no semicolons, data/extension functions, type inference, no checked exceptions)? · 8. What is string interpolation? · 9. What is operator overloading and which keyword is required? · 10. Explain bubble sort. · 11. What's the base case in your recursion?

---

## Practical 2 – Activity Life Cycle & Basic UI (`24012011080_mad_pr2`)

**Aim:** Show "Hello World" centred on a yellow background (Holo Blue Bright, 27sp, bold+italic) and log/Toast/Snackbar every lifecycle callback.

### Files
- `MainActivity.kt` – overrides `onCreate, onStart, onResume, onPause, onStop, onRestart, onDestroy`; each calls `display(msg)`.
- `res/layout/activity_main.xml` – `ConstraintLayout` (id `main`, background `#FFFF00`) containing one `TextView`.
- `AndroidManifest.xml` – declares `MainActivity` with the `MAIN` + `LAUNCHER` intent-filter (so it's the entry screen).

### How it works
- `display()` does three things: `Log.i(TAG, msg)` (Logcat), `Toast.makeText(...)`, and `Snackbar.make(rootView, ...)`.
- The TextView is centred by constraining **all four sides to `parent`** (top, bottom, start, end) with `wrap_content` size.
- `enableEdgeToEdge()` + `setOnApplyWindowInsetsListener` pad the layout so content isn't hidden behind the status/navigation bars.

### Lifecycle sequences (memorise!)
| Action | Callbacks |
|---|---|
| Launch app | `onCreate → onStart → onResume` |
| Press Home | `onPause → onStop` |
| Re-open from Recents | `onRestart → onStart → onResume` |
| Press Back | `onPause → onStop → onDestroy` |
| Rotate screen | `onPause → onStop → onDestroy → onCreate → onStart → onResume` (activity is recreated) |
| Dialog / partial overlay on top | `onPause` only (activity still visible) |

- **Visible** between `onStart`–`onStop`. **In foreground / interactive** between `onResume`–`onPause`.
- `onCreate` = one-time setup (inflate layout). `onDestroy` = release resources.

### Concepts
- Toast vs Snackbar: Toast is a plain system pop-up, no action, not tied to a view. Snackbar is Material, attached to a view, can have an action button (e.g. "Undo") and can be swiped away.
- Logcat levels: `Log.v / d / i / w / e`; the TAG lets you filter.
- ConstraintLayout vs LinearLayout/RelativeLayout: flat hierarchy, positions defined by constraints.
- `sp` for text (scales with user font setting) vs `dp` for sizes.
- `@android:color/holo_blue_bright` – referencing a system colour resource.
- Why `super.onX()` is called – the framework's own work must happen. (Your code calls `display()` *before* `super` in most callbacks – works fine, but conventionally `super` goes first in `onCreate/onStart/onResume/onRestart` and last in `onPause/onStop/onDestroy`.)

### Likely viva questions
Name all lifecycle methods and when each fires · Difference between `onPause` and `onStop` · What happens on rotation and how do you preserve state (`onSaveInstanceState`, `ViewModel`) · Which method is called when the app is restarted after Home · Toast vs Snackbar · What is Logcat · What is the manifest for · What makes an Activity the launcher.

---

## Practical 3 – Implicit & Explicit Intents (`24012011080_MAD_Practicle3`, plus a newer variant in private `24012011080_MAD_Pr3`)

**Aim:** One screen with buttons that launch Browser, Dialer, Call Log, Gallery, Camera, Alarm (all **implicit**) and a custom Login screen (**explicit**).

### Files
- `MainActivity.kt` – `implicitIntent()` and `explicitIntent()` set up the button click listeners.
- `LoginActivity.kt` – second screen: email + password, shows a Toast on Login / "Forgot Password".
- `activity_main.xml` – rows of `TextView` + `EditText`/`Button` in a ConstraintLayout (Web URL, Phone, Call Log, Gallery, Camera, Alarm, Login).
- `activity_login.xml` – logo, card with email/password, button. `drawable/card_background.xml` is the rounded card shape.
- `AndroidManifest.xml` – **every Activity must be registered**; `LoginActivity` has `exported="false"`.

### Intent types
| Button | Intent | Notes |
|---|---|---|
| Browse | `Intent(ACTION_VIEW, Uri.parse(url))` | URL must include `https://` |
| Call | `Intent(ACTION_DIAL)` + `data = "tel:$number".toUri()` | **Opens the dialer only** → no permission needed |
| Call Log | `ACTION_VIEW` + `CallLog.Calls.CONTENT_URI` | |
| Gallery | `ACTION_VIEW` + `setType("image/*")` | MIME type tells Android "any image app" |
| Camera | `MediaStore.ACTION_IMAGE_CAPTURE` | |
| Alarm | `AlarmClock.ACTION_SHOW_ALARMS` | |
| Login | `Intent(this@MainActivity, LoginActivity::class.java)` | **Explicit** – names the exact class |

### Concepts
- **Explicit intent**: you name the target component (class) – used inside your own app.
- **Implicit intent**: you declare an *action* (+ data/type) and Android finds an app whose **intent-filter** matches; if several match, the chooser appears.
- Intent parts: **action, data (URI), type (MIME), category, extras, component, flags**.
- `Uri.parse("tel:…")` – URI schemes: `http(s):`, `tel:`, `mailto:`, `geo:`, `content:`.
- `setData()` and `setType()` – note `setType` *clears* data and vice-versa; use `setDataAndType()` to set both.
- `startActivity()` vs `startActivityForResult()` (now `registerForActivityResult`).
- `putExtra()/getStringExtra()` – passing data between activities (not needed in your Login flow, but be ready).
- `.also { }` / `.apply { }` – Kotlin scope functions (`also` → `it`, `apply` → `this`).

### The newer variant (private repo `Pr3`)
It uses **runtime permissions**: `ActivityResultContracts.RequestPermission()`, `ContextCompat.checkSelfPermission`, `ACTION_CALL` (calls directly → needs `CALL_PHONE`), `READ_CALL_LOG`, `CAMERA`, `GetContent()` for the gallery picker, and `ACTION_SET_ALARM` with `EXTRA_HOUR/MINUTES/MESSAGE` plus a `resolveActivity()` check. Know the difference: **ACTION_DIAL = no permission, ACTION_CALL = needs `CALL_PHONE`**. Tell the examiner which version you're demoing.

### Likely viva questions
Implicit vs explicit intent (with example) · What is an intent-filter · What happens if no app can handle an implicit intent (`ActivityNotFoundException` → check with `resolveActivity`) · ACTION_DIAL vs ACTION_CALL · What are dangerous/runtime permissions · Why register activities in the manifest · What is a MIME type · What does `exported` mean.

---

## Practical 4 – Alarm app with Service + BroadcastReceiver (`24012011080_MAD_Pr4`)

**Aim:** Pick a time, schedule an alarm that rings even if the app is closed, and cancel it.

### Files
- `MainActivity.kt` – UI logic: `TimePickerDialog`, builds a `Calendar`, schedules via `AlarmManager`, cancels.
- `AlarmBroadcastReceiver.kt` – wakes when the alarm fires; starts/stops the service based on the `"Service1"` extra (`"Start"`/`"Stop"`).
- `AlarmService.kt` – plays `res/raw/alarm.mp3` in a loop with `MediaPlayer`.
- `activity_main.xml` – two `MaterialCardView`s (create / cancel), `TextClock` (live clock), `MaterialButton`s.
- `AndroidManifest.xml` – `SCHEDULE_EXACT_ALARM` permission, `<receiver>` and `<service>` registered.

### Flow (draw this on paper in the viva)
```
Create Alarm button
  → TimePickerDialog (hour, minute)
  → Calendar set to that time (if already past → add 1 day)
  → PendingIntent.getBroadcast(Intent(AlarmBroadcastReceiver, extra "Service1"="Start"))
  → AlarmManager.setExact(RTC_WAKEUP, time, pendingIntent)
        ... time passes, app may be closed ...
  → System fires the broadcast → AlarmBroadcastReceiver.onReceive()
  → startService(AlarmService) → onStartCommand: MediaPlayer.create(R.raw.alarm), looping, start()

Cancel Alarm button
  → alarmManager.cancel(pendingIntent) + sendBroadcast(extra "Stop")
  → receiver calls stopService() → AlarmService.onDestroy(): stop() + release()
```

### Concepts
- **Service**: component for work without UI. Types: started (`startService`), bound (`bindService`/`onBind`), foreground. Yours is a *started* service; `onBind` returns `null`.
- Service lifecycle: `onCreate → onStartCommand → (running) → onDestroy`.
- `START_STICKY` – system restarts the service if it gets killed.
- **BroadcastReceiver**: reacts to system/app broadcasts; `onReceive()` must be short. Registered either in the **manifest** (static) or in code (dynamic). Yours is manifest-registered with `exported=false`.
- **AlarmManager**: schedules work at a time, survives the app being closed. `RTC_WAKEUP` = use wall-clock time and wake the device. `setExact` vs `set` (inexact, battery-friendly) vs `setRepeating`.
- **PendingIntent**: a token that lets another process (AlarmManager) fire your intent later with your identity. `FLAG_UPDATE_CURRENT` updates extras; `FLAG_IMMUTABLE` is required on Android 12+. The same request code (`234324243`) is reused so cancel finds the same PendingIntent.
- `Calendar`, `SimpleDateFormat("hh:mm:ss a")`, `TimePickerDialog`, `View.GONE/VISIBLE`.
- Exact-alarm permission: Android 12+ needs the user to allow it (`canScheduleExactAlarms()` → else open `ACTION_REQUEST_SCHEDULE_EXACT_ALARM` settings).
- `MediaPlayer`: `create`, `isLooping`, `start`, `stop`, `release` (always release to free resources).

### Be honest about
- `canScheduleExactAlarms()` exists only from **API 31**; `minSdk` is 24, so on Android 7–11 devices this line would crash. Mention you'd guard it with `Build.VERSION.SDK_INT >= Build.VERSION_CODES.S`.
- On modern Android, long-running background audio should be a **foreground service with a notification**; a plain started service is the textbook/lab approach.
- The "Alarm set for" text only updates when you create an alarm.

### Likely viva questions
Service vs Activity vs BroadcastReceiver · started vs bound service · What is a PendingIntent and why is it needed · Why does the alarm work after the app is closed · What does `RTC_WAKEUP` mean · What is `START_STICKY` · Static vs dynamic receiver registration · How do you cancel an alarm · Why call `release()` on MediaPlayer · Why do we register the receiver/service in the manifest.

---

## Practical 6 – Frame & Tween Animation (`24012011080_MAD_Pr6`)

**Aim:** Splash screen with frame + tween animation, then the main screen with an animated alarm and a heart.

### Files
- `SplashActivity.kt` (**launcher**) – starts the frame animation on the logo and a tween animation; on finish opens `MainActivity`.
- `MainActivity.kt` – starts two frame animations (alarm and heart).
- `res/drawable/uvpce_animation_list.xml` – frame animation (8 logo frames, `oneshot="true"`).
- `res/drawable/alarm_frame_anim.xml` – 10 alarm frames, `oneshot="false"` (loops).
- `res/drawable/ic_heart_outline.xml` – 5 heart-fill frames (0 → 100%), loops.
- `res/anim/twinanimation.xml` – tween `<set>` with **translate, rotate (0→360°), scale up (1→2), scale down (1→0.5)**; `startOffset` delays the start.
- `res/anim/heart_pulse.xml` – a scale-pulse tween (1.0→1.3, reverse, infinite). *It exists but isn't loaded in code – the heart currently animates by frames.*
- `rectangle_gradient.xml` – splash background. `activity_splash.xml`, `activity_main.xml` – layouts. Manifest: `SplashActivity` is the launcher, `MainActivity` `exported=false`.

### How it works
- **Frame animation**: `imageView.setBackgroundResource(R.drawable.xxx)` → `imageView.background as AnimationDrawable` → `.start()` / `.stop()`.
- **Tween animation**: `AnimationUtils.loadAnimation(context, R.anim.twinanimation)` → `view.startAnimation(anim)`.
- `SplashActivity implements Animation.AnimationListener` → `onAnimationEnd()` fires `Intent(this, MainActivity::class.java)`; `onAnimationStart` / `onAnimationRepeat` are empty.

### Key concept: why `onWindowFocusChanged`?
`AnimationDrawable` can't reliably start in `onCreate`/`onStart` because the drawable isn't attached to the window yet. `onWindowFocusChanged(hasFocus = true)` is the standard place to `start()`, and `false` is where you `stop()` to save resources.

### Concepts
- **Frame (drawable) animation** = flipbook of images (`<animation-list>`, `<item android:drawable duration>`). `oneshot` true = play once, false = loop.
- **Tween (view) animation** = transform one view over time (`translate`, `rotate`, `scale`, `alpha`), described in XML under `res/anim/`. Only changes how the view is *drawn*, not its real position/click area.
- `<set>` groups animations; `startOffset`, `duration`, `repeatCount`, `repeatMode` (`restart`/`reverse`), `pivotX/Y` (`50%` = centre), `interpolator`.
- Tween vs Property animation (`ObjectAnimator`/`ValueAnimator`): property animation really changes the property and works on any object.
- Splash screen pattern: launcher activity → animation → start main activity.

### Be honest about
- `SplashActivity` doesn't call `finish()` after launching `MainActivity`, so pressing **Back** on the main screen returns to the splash. Adding `finish()` in `onAnimationEnd` fixes it.
- Your README says the heart uses a scale pulse; the code uses frame animation (the scale XML is unused). Say what the code does.

### Likely viva questions
Frame vs tween animation · What is `AnimationDrawable` · Why `onWindowFocusChanged` · What does `oneshot` mean · Name tween animation types · What is a pivot point · What is `AnimationListener` and its 3 methods · Tween vs property animation · How do you chain animations (`startOffset`, listener) · Where are animation XMLs stored (`res/anim`, `res/drawable`).

---

## Practical 7 – JSON API → SQLite → RecyclerView (`24012011080_MAD_Pr7`)

**Aim:** Fetch person data (JSON) from the internet, store it in SQLite, show it in a list.

### Files
- `MainActivity.kt` – loads from DB; if empty (or FAB tapped) fetches from API; parses JSON; saves to DB; refreshes the list; handles delete.
- `HttpRequest.kt` – `HttpURLConnection` GET with `Authorization: Bearer <token>`; reads stream into a String via `Scanner.useDelimiter("\\A")`.
- `Person.kt` – model (id, name, emailId, phoneNo, address, latitude, longitude), `Serializable`.
- `PersonDbTableData.kt` – table/column names and the `CREATE TABLE` SQL (id is `TEXT PRIMARY KEY`).
- `DatabaseHelper.kt` – `SQLiteOpenHelper`: `onCreate`, `onUpgrade`, `insertPerson`, `updatePerson`, `deletePerson`, `clearAllPersons`, `getPerson`, `allPersons`, `personsCount`.
- `PersonAdapter.kt` – `RecyclerView.Adapter` + `ViewHolder`; delete button calls back to the activity.
- `activity_main.xml` (title, `RecyclerView`, `ProgressBar`, FAB) and `item_person.xml` (a `MaterialCardView` row).
- Manifest: `INTERNET` permission. `build.gradle`: `viewBinding = true`, RecyclerView and coroutines dependencies.

### Flow
```
onCreate → dbHelper.allPersons → adapter shows them
        └ if empty → fetchPersonsFromApi()
fetchPersonsFromApi:
   ProgressBar visible
   CoroutineScope(Dispatchers.IO).launch { HttpRequest().makeServiceCall(url, token) }   // background thread
   withContext(Dispatchers.Main) { parse JSON → clear DB → insert each → reload list → notifyDataSetChanged }
Delete button → DB delete + list.removeAt + notifyItemRemoved
```

### Concepts
- **Network on a background thread**: Android throws `NetworkOnMainThreadException` otherwise. Coroutines: `Dispatchers.IO` for network/DB, `Dispatchers.Main` for UI; `withContext` switches thread.
- **JSON parsing**: `JSONArray` (`[...]`) and `JSONObject` (`{...}`); `getString`, `getDouble`, `optString` (returns "" instead of throwing if missing).
- **SQLite**: `SQLiteOpenHelper` (creates/upgrades DB), `ContentValues` (column→value map), `insertWithOnConflict(..., CONFLICT_REPLACE)`, `update/delete(table, "id = ?", arrayOf(id))` – the `?` placeholders prevent **SQL injection**, `Cursor` (`moveToFirst`, `moveToNext`, `getColumnIndexOrThrow`, **close it**). `DATABASE_VERSION` bump triggers `onUpgrade`.
- **RecyclerView**: recycles row views for performance. Needs a **LayoutManager** (`LinearLayoutManager`), an **Adapter** (`onCreateViewHolder`, `onBindViewHolder`, `getItemCount`) and a **ViewHolder**. `notifyItemRemoved` (animates one row) vs `notifyDataSetChanged` (redraws all).
- **View Binding**: auto-generated class (`ActivityMainBinding`) replaces `findViewById`, null- and type-safe.
- Lambda as callback: `PersonAdapter(persons) { position -> deletePerson(position) }`.
- `companion object` for constants (Kotlin's `static`).
- Offline-first idea: UI always reads from the DB, so the data survives restarts without network.
- HTTP basics: GET, headers, Bearer token auth, JSON, HTTP status codes.

### Be honest about
- The API token is hard-coded in `MainActivity` – fine for a lab, but in real apps secrets don't belong in source.
- `CoroutineScope(Dispatchers.IO)` is not lifecycle-aware; production code uses `lifecycleScope`. Also DB inserts happen on the main thread (inside `withContext(Main)`) – OK for a handful of rows, not for big data.
- `DatabaseHelper` closes the DB after every call (`db.close()`); `SQLiteOpenHelper` normally manages a single connection.
- `HttpRequest` returns `null` on any error (and `HttpURLConnection` doesn't check the status code), which is why the activity shows "Could not fetch contacts".
- `ProgressBar` is hidden before parsing/DB work, which is fast here.

### Likely viva questions
Why can't we do network calls on the main thread · What is a coroutine / `Dispatchers.IO` vs `Main` · What is `SQLiteOpenHelper` and when are `onCreate`/`onUpgrade` called · What is a `Cursor` · What is `ContentValues` · Why use `?` in queries · How does RecyclerView work vs ListView · What is a ViewHolder · What is the INTERNET permission for · JSON object vs array · What is View Binding · Why store in a DB instead of showing the response directly · What does `CONFLICT_REPLACE` do.

---

## Cross-practical themes (examiners love these)

| Theme | Where you used it |
|---|---|
| AndroidManifest (activities, launcher, permissions, receiver, service) | P2, P3, P4, P6, P7 |
| Activity lifecycle | P2 (explicit), P6 (`onWindowFocusChanged`) |
| Intents | P3 (implicit + explicit), P4 (to receiver/service), P6 (splash → main) |
| App components – Activity, Service, BroadcastReceiver | P4 (all three) |
| Layouts: ConstraintLayout, Material cards | P2–P7 |
| Resources: `res/layout, drawable, anim, raw, values` | P3, P4, P6 |
| Threads / background work | P7 (coroutines), P4 (service) |
| Persistence | P7 (SQLite) |
| Permissions | P3 (call/camera/call log), P4 (exact alarm), P7 (internet) |

**Android project structure to be able to sketch:** `manifests/`, `java/<package>/` (Kotlin code), `res/` (`layout`, `drawable`, `values` – strings/colors/themes, `anim`, `raw`, `mipmap`, `xml`), Gradle files (`build.gradle.kts` → `compileSdk`, `minSdk`, `targetSdk`, dependencies, `viewBinding`).

**Four things to say confidently:** Activities have a lifecycle managed by the system · Components talk through Intents · Anything slow (network/DB/audio) goes off the main thread or into a Service · Dangerous operations need manifest + runtime permission.
