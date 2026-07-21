# Stash

Stashes the entire current HTTP request (POST, GET, cookies, files, server vars, headers) into the session under a name, so it can be restored later — the classic "preserve form input across a redirect" pattern.

## Example

```php
use orange\stash\Stash;

$stash = Stash::getInstance($session, $input); // SessionInterface, InputInterface

// on the page handling a failed form submission, before redirecting back:
$stash->push('contact_form');
$output->redirect('/contact');

// back on the form page:
$data = $stash->apply('contact_form');

if ($data !== false) {
    $oldPost = $data['request']; // repopulate form fields from the previous submission
}
```

`apply()` removes the stashed data from the session as soon as it's read, so it can only be restored once; it returns `false` if nothing was stashed under that name. Omit the name to use a single default stash slot.
