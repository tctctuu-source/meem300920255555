import React, { useState } from 'react';
import { supabase } from '../../lib/supabase';
import { defaultPosterStyles } from '../../lib/defaultStyles';
import { Loader2, UploadCloud } from 'lucide-react';

interface PosterTemplateFormProps {
  onClose: () => void;
  onSave: () => void;
}

const PosterTemplateForm: React.FC<PosterTemplateFormProps> = ({ onClose, onSave }) => {
  const [name, setName] = useState('');
  const [file, setFile] = useState<File | null>(null);
  const [isSubmitting, setIsSubmitting] = useState(false);
  const [error, setError] = useState<string | null>(null);
  const [preview, setPreview] = useState<string | null>(null);

  const handleFileChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    if (e.target.files && e.target.files[0]) {
      const selectedFile = e.target.files[0];
      setFile(selectedFile);
      const reader = new FileReader();
      reader.onloadend = () => {
        setPreview(reader.result as string);
      };
      reader.readAsDataURL(selectedFile);
    }
  };

  const handleSubmit = async (e: React.FormEvent) => {
    e.preventDefault();
    if (!file || !name) {
      setError('Template name and a background image are required.');
      return;
    }
    setIsSubmitting(true);
    setError(null);

    try {
      // 1. Upload image to Supabase Storage
      const filePath = `public/${Date.now()}-${file.name}`;
      const { error: uploadError } = await supabase.storage
        .from('poster_backgrounds')
        .upload(filePath, file);
      
      if (uploadError) throw uploadError;

      // 2. Get public URL of the uploaded image
      const { data: { publicUrl } } = supabase.storage
        .from('poster_backgrounds')
        .getPublicUrl(filePath);

      // 3. Insert new template into the database
      const { error: insertError } = await supabase
        .from('poster_templates')
        .insert({
          name,
          layout_name: 'FlexibleLayout',
          background_image_url: publicUrl,
          thumbnail_url: publicUrl, // Use the same image for thumbnail
          styles: defaultPosterStyles['FlexibleLayout'],
          is_active: true,
        });

      if (insertError) throw insertError;

      onSave();
      onClose();

    } catch (err: any) {
      setError(`Failed to create template: ${err.message}`);
      console.error(err);
    } finally {
      setIsSubmitting(false);
    }
  };

  return (
    <div className="fixed inset-0 bg-black/60 z-[60] flex items-center justify-center p-4">
      <div className="bg-white rounded-lg p-8 w-full max-w-lg shadow-2xl">
        <h3 className="text-xl font-bold text-gray-800 mb-6">Create Custom Poster Template</h3>
        
        <form onSubmit={handleSubmit} className="space-y-6">
          {error && <div className="bg-rose-100 text-rose-700 p-3 rounded-md text-sm">{error}</div>}
          
          <div>
            <label htmlFor="name" className="block text-sm font-medium text-gray-700 mb-1">Template Name</label>
            <input
              type="text"
              id="name"
              value={name}
              onChange={(e) => setName(e.target.value)}
              className="w-full px-3 py-2 border border-gray-300 rounded-lg"
              placeholder="e.g., 'Blue Abstract Design'"
              required
            />
          </div>

          <div>
            <label className="block text-sm font-medium text-gray-700 mb-1">Background Image</label>
            <div className="mt-2 flex justify-center px-6 pt-5 pb-6 border-2 border-gray-300 border-dashed rounded-md">
              <div className="space-y-1 text-center">
                {preview ? (
                  <img src={preview} alt="Background preview" className="mx-auto h-24 w-auto rounded-md" />
                ) : (
                  <UploadCloud className="mx-auto h-12 w-12 text-gray-400" />
                )}
                <div className="flex text-sm text-gray-600">
                  <label htmlFor="file-upload" className="relative cursor-pointer bg-white rounded-md font-medium text-blue-600 hover:text-blue-500 focus-within:outline-none">
                    <span>Upload a file</span>
                    <input id="file-upload" name="file-upload" type="file" className="sr-only" onChange={handleFileChange} accept="image/png, image/jpeg, image/webp" required />
                  </label>
                  <p className="pl-1">or drag and drop</p>
                </div>
                <p className="text-xs text-gray-500">PNG, JPG, WEBP up to 2MB</p>
              </div>
            </div>
          </div>

          <div className="flex items-center justify-end space-x-4 pt-4">
            <button type="button" onClick={onClose} className="bg-gray-200 text-gray-800 py-2 px-4 rounded-lg hover:bg-gray-300">
              Cancel
            </button>
            <button
              type="submit"
              disabled={isSubmitting}
              className="bg-blue-600 text-white py-2 px-6 rounded-lg hover:bg-blue-700 disabled:opacity-50 flex items-center gap-2"
            >
              {isSubmitting ? <Loader2 className="animate-spin" /> : 'Create Template'}
            </button>
          </div>
        </form>
      </div>
    </div>
  );
};

export default PosterTemplateForm;
