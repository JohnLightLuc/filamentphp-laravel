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

### Ajout enregistrement d'images

     Forms\Components\FileUpload::make('documents')
                    ->label('Documents')
                    ->multiple()
                    ->directory('documents') // Le dossier de stockage dans storage/app/public
                    ->visibility('public')
                    ->downloadable()
                    ->openable()
                    ->reorderable()
                    ->maxSize(10240) // 10MB max par fichier
                    ->acceptedFileTypes(['application/pdf', 'image/*', 'application/msword', 
                        'application/vnd.openxmlformats-officedocument.wordprocessingml.document'])
                    ->storeFileNamesIn('document_names') // Optionnel: stocke les noms originaux
                    ->columnSpanFull()
                    ->saveRelationshipsUsing(function ($record, $state) {
                        // Prépare les données pour le stockage JSON
                        $filesData = collect($state)->map(function ($file) {
                            return [
                                'path' => $file,
                                'original_name' => basename($file),
                                'size' => Storage::size('public/' . $file),
                                'mime_type' => Storage::mimeType('public/' . $file),
                                'uploaded_at' => now()->toDateTimeString(),
                            ];
                        })->toArray();

                        $record->update([
                            'documents' => $filesData
                        ]);
                    }),
            ]);

###  Exemple de  modifccation  avant l'enregistrement d'un  item
        class CreateDriver extends CreateRecord
        {
            protected function mutateFormDataBeforeCreate(array $data): array
            {
                // Concaténation des champs dial_code et phone_number
                $data['phone'] = $data['dial_code'] . $data['phone_number'];
        
                return $data;
            }
        }


