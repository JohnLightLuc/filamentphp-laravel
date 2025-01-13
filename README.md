# filamentphp-laravel

[Filament Docs](https://filamentphp.com/docs/3.x/panels/installation)


### Ajouter une  liste de checkbox au  formulaire


    class ProjectResource extends Resource
    {
        protected static ?string $model = Project::class;
    
        public static function form(Form $form): Form
        {
            return $form
                ->schema([
                    // Autres champs du formulaire...
                    
                    Forms\Components\CheckboxList::make('services')
                        ->label('Services disponibles')
                        ->options(function () {
                            return Service::query()
                                ->pluck('name', 'id')
                                ->toArray();
                        })
                        ->columns(2)
                        ->searchable()
                        ->bulkToggleable()
                        ->descriptions(function () {
                            return Service::query()
                                ->pluck('description', 'id')
                                ->toArray();
                        })
                        ->required()
                ]);
        }
    }


