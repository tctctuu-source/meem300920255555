import React, { useState, useEffect } from 'react';
import { Plus, Edit, Trash2, Loader2, Info, AlertTriangle } from 'lucide-react';
import { supabase } from '../../lib/supabase';
import { PosterTemplate } from '../../types';
import PosterEditor from './PosterEditor';
import PosterTemplateForm from './PosterTemplateForm';

const AdminPosters: React.FC = () => {
  const [templates, setTemplates] = useState<PosterTemplate[]>([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<string | null>(null);
  const [editingTemplate, setEditingTemplate] = useState<PosterTemplate | null>(null);
  const [showCreateForm, setShowCreateForm] = useState(false);

  const fetchTemplates = async () => {
    setLoading(true);
    const { data, error } = await supabase
      .from('poster_templates')
      .select('*')
      .order('created_at', { ascending: false });

    if (error) {
      setError("Could not fetch poster templates. Please check your Supabase connection.");
    } else {
      setTemplates(data as PosterTemplate[] || []);
    }
    setLoading(false);
  };

  useEffect(() => {
    fetchTemplates();
  }, []);

  const handleToggleActive = async (template: PosterTemplate) => {
    const updatedTemplates = templates.map(t =>
      t.id === template.id ? { ...t, is_active: !t.is_active } : t
    );
    setTemplates(updatedTemplates);

    const { error } = await supabase
      .from('poster_templates')
      .update({ is_active: !template.is_active })
      .eq('id', template.id);

    if (error) {
      alert(`Error updating status. Details: ${error.message}`);
      await fetchTemplates(); // Revert on error
    }
  };

  const handleDelete = async (id: string, backgroundUrl: string | null) => {
    if (confirm('Are you sure you want to delete this template? This cannot be undone.')) {
      // First, delete from the database
      const { error: dbError } = await supabase.from('poster_templates').delete().eq('id', id);
      
      if (dbError) {
        alert(`Error deleting template from database. Details: ${dbError.message}`);
        return;
      }
      
      // If DB deletion is successful, delete the background image from storage
      if (backgroundUrl) {
        try {
            const urlParts = backgroundUrl.split('/');
            const bucketAndPath = urlParts.slice(urlParts.indexOf('poster_backgrounds')).join('/');
            const filePath = bucketAndPath.substring(bucketAndPath.indexOf('/') + 1);

            if (filePath) {
                const { error: storageError } = await supabase.storage.from('poster_backgrounds').remove([filePath]);
                if (storageError) {
                    alert(`Template deleted, but failed to delete background image. Manual cleanup may be needed. Error: ${storageError.message}`);
                }
            }
        } catch (e) {
            console.error("Could not parse file path from URL for deletion:", backgroundUrl);
        }
      }

      await fetchTemplates();
    }
  };

  return (
    <div className="space-y-6">
      <div className="flex items-center justify-between">
        <div>
          <h2 className="text-2xl font-bold text-ui-text-primary">Manage Poster Models</h2>
          <p className="text-ui-text-secondary">Activate, edit styles, or create new result poster designs.</p>
        </div>
        <button
          onClick={() => setShowCreateForm(true)}
          className="bg-brand-mid-blue text-white px-4 py-2 rounded-lg hover:bg-brand-dark-blue transition-colors flex items-center gap-2"
        >
          <Plus className="w-5 h-5" />
          Add Template
        </button>
      </div>

      <div className="bg-blue-50 border-l-4 border-blue-400 text-blue-800 p-4" role="alert">
        <div className="flex">
          <div className="py-1"><Info className="w-5 h-5 mr-3" /></div>
          <div>
            <p className="font-bold">How it works</p>
            <p className="text-sm">Click "Add Template" to upload a background and create a new design. Use the toggle to activate or deactivate designs, and "Edit Styles" to customize fonts, colors, and layout.</p>
          </div>
        </div>
      </div>

      {loading ? (
        <div className="flex justify-center items-center py-12"><Loader2 className="w-8 h-8 text-brand-mid-blue animate-spin" /></div>
      ) : error ? (
        <div className="text-center py-12 text-rose-600"><h3 className="text-xl font-semibold">Error loading templates</h3><p>{error}</p></div>
      ) : (
        <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-6">
          {templates.map((template) => (
            <div key={template.id} className="bg-ui-surface rounded-lg shadow-md overflow-hidden flex flex-col">
              <div className="relative">
                <img src={template.background_image_url || template.thumbnail_url || 'https://img-wrapper.vercel.app/image?url=https://img-wrapper.vercel.app/image?url=https://img-wrapper.vercel.app/image?url=https://img-wrapper.vercel.app/image?url=https://placehold.co/400x300/F0F5FA/082026?text=No+Preview'} alt={template.name} className="w-full h-40 object-cover bg-gray-200" />
                {!template.layout_name && (
                  <div className="absolute top-2 right-2 bg-amber-400 text-black p-1.5 rounded-full" title="Layout is missing!">
                    <AlertTriangle size={16} />
                  </div>
                )}
              </div>
              <div className="p-4 flex-grow flex flex-col">
                <h3 className="font-bold text-lg text-ui-text-primary">{template.name}</h3>
                <p className="text-sm text-ui-text-secondary mb-3">
                  Layout: {template.layout_name ? 
                    <code className="bg-black/5 px-1 py-0.5 rounded text-xs">{template.layout_name}</code> : 
                    <span className="text-amber-600 font-bold">Missing!</span>
                  }
                </p>
                
                <button
                  onClick={() => setEditingTemplate(template)}
                  className="w-full bg-brand-mid-blue text-white py-2 rounded-lg flex items-center justify-center gap-2 hover:bg-brand-dark-blue transition-colors mb-3"
                >
                  <Edit size={16} />
                  Edit Styles
                </button>

                <div className="mt-auto pt-4 border-t border-black/5 flex items-center justify-between">
                  <div className="flex items-center">
                    <label htmlFor={`toggle-${template.id}`} className="flex items-center cursor-pointer">
                      <div className="relative">
                        <input type="checkbox" id={`toggle-${template.id}`} className="sr-only" checked={template.is_active} onChange={() => handleToggleActive(template)} />
                        <div className="block bg-gray-300 w-12 h-6 rounded-full"></div>
                        <div className={`dot absolute left-1 top-1 bg-white w-4 h-4 rounded-full transition-transform ${template.is_active ? 'translate-x-6 bg-brand-coral' : ''}`}></div>
                      </div>
                      <div className="ml-3 text-sm font-medium text-ui-text-secondary">{template.is_active ? 'Active' : 'Inactive'}</div>
                    </label>
                  </div>
                  <button onClick={() => handleDelete(template.id, template.background_image_url)} className="text-rose-600 hover:text-rose-800 p-2 rounded-full hover:bg-rose-50" title="Delete Template">
                    <Trash2 size={18} />
                  </button>
                </div>
              </div>
            </div>
          ))}
        </div>
      )}

      {showCreateForm && (
        <PosterTemplateForm 
          onClose={() => setShowCreateForm(false)}
          onSave={fetchTemplates}
        />
      )}

      {editingTemplate && (
        <PosterEditor
          template={editingTemplate}
          onClose={() => setEditingTemplate(null)}
          onSave={fetchTemplates}
        />
      )}
    </div>
  );
};

export default AdminPosters;
