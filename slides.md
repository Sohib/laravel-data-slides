---
theme: default
class: text-center
highlighter: shiki
lineNumbers: false
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
drawings:
  persist: false
transition: fade-out
css: unocss

fonts:
  sans: 'Cairo'
  serif: Cairo
  mono: Fira Code

title: مكتبة Laravel-data
htmlAttrs: 
  dir: auto
  lang: ar
---

# مكتبة Laravel-data

## مكتبة البيانات

بدأت المكتبة في فريق سباتي بدل اختصار للوقت في احد مشاريعهم حيث كان يحتاجون كل فترة يكتبون
<div class="text-start">

- Form Request
- Api Resource  
- Typescript Definition

</div>
  مكتبة البيانات ستقوم باستبدال جميع هذه الاشياء

---
layout: two-cols
---

## إستقبل البيانات و تحقق منها

### شفرة للتحقق من صحة نموذج

<div dir=ltr>

```php
    public function update(Organization $organization)
    {
        $organization->update(
            Request::validate([
                'name' => ['required', 'max:100'],
                'email' => ['nullable', 'max:50', 'email'],
                'phone' => ['nullable', 'max:50'],
            ])
        );

        return Redirect::back()
                   ->with('success', 'Organization updated.');
    }
```

</div>

نقوم بإنشاء الكلاس الخاص  بالنموذج الذي نرغب بالتحقق منه

- we extend Data Object from spait
- declare our property names matching our form
- add php 8 attribute (Annoitation )

<div dir=ltr>

```php
class OrganizationData extends Data
{
    public function __construct(
        //
        #[Required, Max(100)]
        public $name,
        #[Nullable, Max(100),Email]
        public $email,
        #[Nullable,Max(50)]
        public $phone,
    )
    {
    }
}

```

</div>

النتيجة كود اقصر بشكل كبير

<div dir=ltr>

```php
    public function update(Organization $organization,OrganizationData $data)
    {
        $organization->update($data);
        return Redirect::back()
                   ->with('success', 'Organization updated.');
    }
```

</div>
---
---

### تمثيل البيانات بواسطة لل view 
سواء REST Api or template engin  

<div dir=ltr>

```php
class OrganizationsController extends Controller {

    // ..  
    // ..

     public function edit(Organization $organization)
    {
        return Inertia::render('Organizations/Edit', [
            'organization' => [
                'id' => $organization->id,
                'name' => $organization->name,
                'email' => $organization->email,
                'phone' => $organization->phone,
            ],
        ]);
    }
}
```

</div>


<div dir=ltr>

```php
class OrganizationsController extends Controller {

    // ..  
    // ..

     public function edit(Organization $organization)
    {
        return Inertia::render('Organizations/Edit', [
            'organization' =>  OrganizationData::fromModel($organization),
        ]);
    }
}
```

</div>

 